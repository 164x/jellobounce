<script lang="ts">
    import ButtonSetting from "../common/setting/ButtonSetting.svelte";
    import {
        directLoginToCrackedAccount,
        getAccounts,
        loginToAccount,
        reconnectToServer
    } from "../../../integration/rest";
    import {faker} from "@faker-js/faker";
    import type {AccountManagerLoginEvent} from "../../../integration/events";
    import {listen} from "../../../integration/ws";
    import {onMount} from "svelte";
    import type {Account} from "../../../integration/types";
    import {
        restoreSession,
    } from "../../../integration/rest.js";
    import Dot from "../Dot.svelte";

    let premiumAccounts: Account[] = [];

    async function reconnectWithRandomUsername() {
        const username = faker.internet.userName().substring(0, 16).replace(/[^a-zA-Z0-9_]+/gi, "");
        await directLoginToCrackedAccount(username);
    }

    async function reconnectWithRandomAccount() {
        const account = premiumAccounts[Math.floor(Math.random() * premiumAccounts.length)];
        await loginToAccount(account.id);
    }

    onMount(async () => {
        premiumAccounts = (await getAccounts()).filter(a => a.type !== "Cracked" && !a.favorite);

        setTimeout(() => { // TODO: Hacky fix for issues caused by stuck route fix
            listen("accountManagerLogin", async (e: AccountManagerLoginEvent) => {
                await reconnectToServer();
            });
        }, 1000);
    });
</script>

<Dot/>
<div class="reconnect">
    <ButtonSetting title="Reconnect" on:click={() => reconnectToServer()}/>
    <ButtonSetting title="Restore initial session" on:click={restoreSession}/>
    <ButtonSetting title="Reconnect with random account" on:click={reconnectWithRandomAccount}
                   disabled={premiumAccounts.length === 0}/>
    <ButtonSetting title="Reconnect with random username" on:click={reconnectWithRandomUsername}/>
</div>

<style lang="scss">
  .reconnect {
    position: fixed;
    bottom: 20px;
    left: 45px;
    display: flex;
    flex-direction: column;
    row-gap: 10px;
    align-items: flex-start;
  }
</style>