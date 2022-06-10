<svelte:options tag="message-modal" />

<script>
    import { get_current_component } from "svelte/internal";
    export let message = "";
    export let type = "success";
    export let modal = true;

    const component = get_current_component();
    let close = () => {
        component.dispatchEvent &&
            component.dispatchEvent(new CustomEvent("close"));
    };

    $: titleColor =
        type == "success"
            ? "has-background-success has-text-white"
            : "has-background-danger has-text-white";
</script>

<div class="modal" class:is-active={modal}>
    <div class="modal-background" />
    <div class="modal-content">
        <div class="card">
            <div class="card-header p-2 {titleColor}">
                <h2 class="is-size-4">訊息</h2>
            </div>
            <div class="card-content p-5">
                <p class="is-size-6">
                    {message}
                </p>
            </div>
            <div class="card-footer p-2 has-text-right">
                <button
                    class="button has-background-primary-light"
                    on:click={close}>關閉</button
                >
            </div>
        </div>
    </div>
</div>

<style>
    @import "../../javascript/bundle.css";
</style>
