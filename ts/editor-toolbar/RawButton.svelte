<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="typescript">
    import { onMount, createEventDispatcher } from "svelte";

    export let html: string;

    const dispatch = createEventDispatcher();
    onMount(() => dispatch("mount", { button: null }));
</script>

{@html html}
