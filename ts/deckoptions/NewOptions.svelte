<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="ts">
    import * as tr from "lib/i18n";
    import SpinBox from "./SpinBox.svelte";
    import SpinBoxFloat from "./SpinBoxFloat.svelte";
    import CheckBox from "./CheckBox.svelte";
    import StepsInput from "./StepsInput.svelte";
    import EnumSelector from "./EnumSelector.svelte";
    import type { DeckOptionsState } from "./lib";

    export let state: DeckOptionsState;
    let config = state.currentConfig;
    let defaults = state.defaults;

    const newOrderChoices = [
        tr.schedulingShowNewCardsInOrderAdded(),
        tr.schedulingShowNewCardsInRandomOrder(),
    ];

    let stepsExceedGraduatingInterval: string;
    $: {
        const lastLearnStepInDays = $config.learnSteps.length
            ? $config.learnSteps[$config.learnSteps.length - 1] / 60 / 24
            : 0;
        stepsExceedGraduatingInterval =
            lastLearnStepInDays > $config.graduatingIntervalGood
                ? tr.deckConfigLearningStepAboveGraduatingInterval()
                : "";
    }

    $: goodExceedsEasy =
        $config.graduatingIntervalGood > $config.graduatingIntervalEasy
            ? tr.deckConfigGoodAboveEasy()
            : "";
</script>

<div>
    <h2>New Cards</h2>

    <StepsInput
        label="Learning steps"
        tooltip="Learning steps, separated by spaces."
        defaultValue={defaults.learnSteps}
        value={$config.learnSteps}
        on:changed={(evt) => ($config.learnSteps = evt.detail.value)} />

    <EnumSelector
        label={tr.schedulingOrder()}
        tooltip=""
        choices={newOrderChoices}
        defaultValue={defaults.newCardOrder}
        bind:value={$config.newCardOrder} />

    <SpinBox
        label={tr.schedulingGraduatingInterval()}
        tooltip="Days to wait after answering Good on the last learning step."
        warnings={[stepsExceedGraduatingInterval]}
        defaultValue={defaults.graduatingIntervalGood}
        bind:value={$config.graduatingIntervalGood} />

    <SpinBox
        label={tr.schedulingEasyInterval()}
        tooltip="Days to wait after answering Easy on the first learning step."
        warnings={[goodExceedsEasy]}
        defaultValue={defaults.graduatingIntervalEasy}
        bind:value={$config.graduatingIntervalEasy} />

    <SpinBoxFloat
        label={tr.schedulingStartingEase()}
        tooltip="The default multiplier when a review is answered Good."
        min={1.31}
        max={5}
        defaultValue={defaults.initialEase}
        value={$config.initialEase}
        on:changed={(evt) => ($config.initialEase = evt.detail.value)} />

    <CheckBox
        label={tr.schedulingBuryRelatedNewCardsUntilThe()}
        defaultValue={defaults.buryNew}
        bind:value={$config.buryNew} />
</div>
