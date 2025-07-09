<script lang="ts">
    import type { CollectionEntry } from "astro:content";
    export let items: CollectionEntry<'sections'>[];
    let selectedItem: CollectionEntry<'sections'> = items[5];

    function getIndexDistanceToSelectedItem(index: number): number {
        let selectedItemIndex = items.findIndex(item => item.data.title === selectedItem.data.title);
        return Math.abs(selectedItemIndex - index);
    }

    function mapValue(value: number, inputMin: number, inputMax: number, outputMin: number, outputMax: number): number {
        return ((value - inputMin) / (inputMax - inputMin)) * (outputMax - outputMin) + outputMin;
    }
</script>

<div class="flex flex-col justify-between p-8 text-right text-primary-foreground bg-primary">
    <h1 class="text-xl">Name Lastname</h1>
    <div class="flex flex-col gap-4 text-4xl">
        {#each items as item, index}
            <button onclick={() => { selectedItem = item; }} 
                style={"opacity: " + mapValue((items.length - getIndexDistanceToSelectedItem(index)), 0, items.length, 0.3, 1) + "; " +
                        "font-size: " + mapValue((items.length - getIndexDistanceToSelectedItem(index)), 0, items.length, 0.5, 2.5) + "rem"}>
                {item.data.title}
            </button>
        {/each}
    </div>
    <p class="text-sm opacity-60">Site design & development by Kadir Lofca.</p>
</div>
<!-- 
                style={"opacity: " + mapValue(Math.pow(items.length - getIndexDistanceToSelectedItem(index), 4), 0, Math.pow(items.length, 4), 0.3, 1) + "; " +
                        "font-size: " + mapValue(Math.pow(items.length - getIndexDistanceToSelectedItem(index), 4), 0, Math.pow(items.length, 4), 0.5, 2.5) + "rem"}> -->