<svelte:options tag="confirm-modal" />

<script>
    import { get_current_component } from "svelte/internal";
    export let title = "";
    export let content = "";
    export let modal = false;
    let reason = "";

    $: canConfirm = !!reason;

    const component = get_current_component();

    let confirm = () => {
        component.dispatchEvent &&
            component.dispatchEvent(
                new CustomEvent("confirm", { detail: { reason } })
            );
        reason = "";
    };
    let cancel = () => {
        // * 如果是custom component , 要丟事件出去,要用這段
        component.dispatchEvent &&
            component.dispatchEvent(new CustomEvent("cancel"));
        reason = "";
    };
</script>

<div class="modal" class:is-active={modal}>
    <div class="modal-background" />
    <div class="modal-content">
        <div class="card">
            <div class="card-header p-2 has-background-grey-lighter">
                <h2 class="is-size-4">{title}</h2>
            </div>
            <div class="card-content p-5">
                <textarea
                    bind:value={reason}
                    class="textarea"
                    placeholder="請輸入取消借用原由"
                    maxlength="250"
                />
                <hr />
                <p class="is-size-6">
                    {@html content}
                </p>
            </div>
            <div class="card-footer p-2 has-text-right">
                <button
                    disabled={!canConfirm}
                    class="button has-background-danger-dark has-text-white"
                    on:click={confirm}>確定</button
                >
                <button
                    class="button ml-2 has-background-link-light"
                    on:click={cancel}>取消</button
                >
            </div>
        </div>
    </div>
</div>

<style>
    @import "../../javascript/bundle.css";
</style>
