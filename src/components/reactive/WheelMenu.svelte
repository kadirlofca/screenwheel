<script lang="ts">
    import type { CollectionEntry } from "astro:content";
    export let items: CollectionEntry<'sections'>[];

    function getIndexDistanceToSelectedNavigationLink(navigationLink: string): number {
        return 0;
    }
</script>

<div class="flex flex-col justify-between p-8 text-right text-primary-foreground bg-primary">
    <h1 class="text-xl">Name Lastname</h1>
    <div class="flex flex-col gap-4 text-4xl">
        {#each items as item}
            <span>{item.data.title}</span>
        {/each}
    </div>
    <p class="text-sm opacity-60">Site design & development by Kadir Lofca.</p>
</div>