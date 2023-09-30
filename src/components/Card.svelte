<script lang="ts">
    import type { Rank } from "../types/Rank";
    import type { Suit } from "../types/Suit";

    export let suit: Suit | undefined;
    export let rank: Rank | undefined;

    function getCardRank() {}
</script>

<div class="card-border">
    <p>{rank}</p>
    <p>{suit}</p>
</div>

<style>
    .card-border {
        border: 2px solid white;
        border-radius: 5px;
        width: 10vh;
        height: 14vh;
    }
</style>
