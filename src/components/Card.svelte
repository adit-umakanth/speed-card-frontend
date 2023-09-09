<script lang="ts">
    export let suit: string;
    export let rank: string;
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
