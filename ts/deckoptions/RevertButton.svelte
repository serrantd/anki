<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="ts">
    import { revertIcon } from "./icons";
    import { createEventDispatcher } from "svelte";
    import { isEqual, cloneDeep } from "lodash-es";

    export let value: any;
    export let defaultValue: any;

    const dispatch = createEventDispatcher();

    let modified: boolean;
    $: modified = !isEqual(value, defaultValue);

    /// This component can be used either with bind:value, or by listening
    /// to the revert event.
    function revert(): void {
        value = cloneDeep(defaultValue);
        dispatch("revert", { value });
    }
</script>

<style lang="scss">
    .img-div {
        display: flex;

        :global(svg) {
            align-self: center;
            opacity: 0.3;
        }
    }
</style>

{#if modified}
    <div class="img-div" on:click={revert}>
        {@html revertIcon}
    </div>
{/if}
