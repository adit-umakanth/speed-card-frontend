<script>
    import Card from "./Card.svelte";
    import CardBack from "./CardBack.svelte";

    let websocket = new WebSocket("ws://127.0.0.1:8080");
    websocket.onmessage = (event) => {
        console.log(event.data);
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
    <div style="grid-area: 3 / 1 / 4 / 2;">
        <CardBack />
    </div>
    <div style="grid-area: 3 / 6 / 4 / 7; ">
        <CardBack />
    </div>
    <!-- Active Cards -->
    <div style="grid-area: 3 / 3 / 4 / 4;">
        <Card suit="Spades" rank="Ace" />
    </div>
    <div style="grid-area: 3 / 4 / 4 / 5;">
        <Card suit="Hearts" rank="King" />
    </div>
    <!-- Player Hand-->
    <div class="player-hand" style="grid-area: 4 / 2 / 5 / 6;">
        <CardBack />
        <CardBack />
        <CardBack />
        <CardBack />
    </div>
    <!-- Player Pile -->
    <div style="grid-area: 5 / 6 / 6 / 7;">
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
