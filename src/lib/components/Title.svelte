<script>
    import { accentColor } from "$lib/utils.js";

    let { classes, text, type = "" } = $props();
</script>

{#if type === "sameLine"}
    <div class="flex items-center gap-2">
        <span class={classes} style={`color:${accentColor};`}>{text}</span>
        <div class="flex-1 h-px" style={`background:${accentColor};`}></div>
    </div>
{:else if type === "newLine"}
    <div class="flex items-center gap-2 border-bottom mb-1" style={`border-bottom: 1px solid ${accentColor};`}>
        <span style={`color:${accentColor};`} class={classes}>{text}</span>
    </div>
{:else}
    <span style={`color:${accentColor};`} class={classes}>{text}</span>
{/if}
