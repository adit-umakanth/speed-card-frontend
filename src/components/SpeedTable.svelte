<script lang="ts">
    import type { PlayerView } from "../types/PlayerView";
    import type { ServerMessage } from "../types/ServerMessage";
    import Card from "./Card.svelte";
    import CardBack from "./CardBack.svelte";

    let player_view: PlayerView = {
        active_cards: [null, null],
        middle_piles: [false, false],
        opponent_hand: [false, false, false, false],
        opponent_pile: false,
        player_hand: [null, null, null, null],
    };

    $: ({
        active_cards,
        middle_piles,
        opponent_hand,
        opponent_pile,
        player_hand,
    } = player_view);

    let websocket = new WebSocket("ws://127.0.0.1:8080");
    websocket.onmessage = (event) => {
        let message: ServerMessage = JSON.parse(event.data);
        console.log(message);
        player_view = message.player_view;
    };
</script>

<div class="speedtable">
    <!-- Opponent Pile -->
    <CardBack />
    <!-- Opponent Hand -->
    <div class="player-hand" style="grid-area: 2 / 2 / 3 / 6;">
        <CardBack />
        <CardBack />
        <CardBack />
        <CardBack />
    </div>
    <!-- Middle Flip Cards -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        style="grid-area: 3 / 1 / 4 / 2;"
        on:click={(e) => websocket.send(JSON.stringify("Flip"))}
    >
        <CardBack />
    </div>
    <div style="grid-area: 3 / 6 / 4 / 7; ">
        <CardBack />
    </div>
    <!-- Active Cards -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        on:drop={(e) => {
            e.preventDefault();
            let ind = parseInt(e.dataTransfer?.getData("text/plain") || "0");
            console.log({ PlaceCard: [ind, "LEFT"] });
            websocket.send(JSON.stringify({ PlaceCard: [ind, "LEFT"] }));
        }}
        on:dragover={(e) => e.preventDefault()}
        style="grid-area: 3 / 3 / 4 / 4;"
    >
        <Card suit={active_cards[0]?.suit} rank={active_cards[0]?.rank} />
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        on:drop={(e) => {
            e.preventDefault();
            let ind = parseInt(e.dataTransfer?.getData("text/plain") || "0");
            console.log({ PlaceCard: [ind, "RIGHT"] });
            websocket.send(JSON.stringify({ PlaceCard: [ind, "RIGHT"] }));
        }}
        on:dragover={(e) => e.preventDefault()}
        style="grid-area: 3 / 4 / 4 / 5;"
    >
        <Card suit={active_cards[1]?.suit} rank={active_cards[1]?.rank} />
    </div>
    <!-- Player Hand-->
    <div class="player-hand" style="grid-area: 4 / 2 / 5 / 6;">
        {#each player_hand as player_card, i}
            {#if player_card == null}
                <CardBack />
            {:else}
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div
                    draggable="true"
                    on:dragstart={(e) =>
                        e.dataTransfer?.setData("text/plain", i.toString())}
                >
                    <Card suit={player_card.suit} rank={player_card.rank} />
                </div>
            {/if}
        {/each}
    </div>
    <!-- Player Pile -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        style="grid-area: 5 / 6 / 6 / 7;"
        on:click={(e) => websocket.send(JSON.stringify("DrawCard"))}
    >
        <CardBack />
    </div>
</div>

<style>
    .speedtable {
        display: grid;
        justify-items: center;
        align-items: center;
        grid-gap: 5px;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: repeat(5, 1fr);
        min-width: 100vw;
        min-height: 100vh;
    }
    .player-hand {
        display: flex;
        margin: auto;
    }
</style>
