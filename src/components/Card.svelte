<script lang="ts">
    import type { Rank } from "../types/Rank";
    import type { Suit } from "../types/Suit";

    import diamondSign from "../assets/diamonds.svg";
    import heartSign from "../assets/hearts.svg";
    import spadeSign from "../assets/spades.svg";
    import clubSign from "../assets/clubs.svg";

    import cardMap from "../lib/card_map";

    export let suit: Suit | undefined;
    export let rank: Rank | undefined;

    let suitMap: Record<Suit, string> = {
        Clubs: clubSign,
        Diamonds: diamondSign,
        Spades: spadeSign,
        Hearts: heartSign,
    };
</script>

<div class="card-border">
    {#if rank !== undefined && suit !== undefined}
        <div class="top-rank">
            {cardMap.card_map[rank]}
            <img src={suitMap[suit]} alt="" style="height: 2vh;" />
        </div>
        <div class="bottom-rank">
            {cardMap.card_map[rank]}
            <img src={suitMap[suit]} alt="" style="height: 2vh;" />
        </div>
    {/if}
</div>

<style>
    .card-border {
        background-color: white;
        border: 2px solid white;
        border-radius: 5px;
        color: black;
        width: 10vh;
        height: 14vh;
        display: grid;
        margin: 10px;
        grid-template-columns: 1fr 2fr 1fr;
        grid-template-rows: 1fr 2fr 1fr;
        grid-column-gap: 0px;
        grid-row-gap: 0px;
    }

    .top-rank {
        display: flex;
        flex-direction: column;
    }

    .bottom-rank {
        display: flex;
        flex-direction: column;
        rotate: 180deg;
        grid-column-start: 3;
        grid-row-start: 3;
    }
</style>
