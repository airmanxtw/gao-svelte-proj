<svelte:options tag="confirm-modal"/>
<script>    
    import { get_current_component } from "svelte/internal"
    export let title="";
    export let content=""
    export let modal=false;

    const component = get_current_component();

    let confirm = () =>{
        component.dispatchEvent && component.dispatchEvent(new CustomEvent('confirm'));
    }
    let cancel = () => {                             
        // * 如果是custom component , 要丟事件出去,要用這段
	    component.dispatchEvent && component.dispatchEvent(new CustomEvent('cancel'));
    }

</script>

<div class="modal" class:is-active={modal}>
    <div class="modal-background" />
    <div class="modal-content">
        <div class="card">
            <div class="card-header p-2 has-background-grey-lighter">
                <h2 class="is-size-4">{title}</h2>
            </div>
            <div class="card-content is-size-6 p-2">
                {@html content}
            </div>
            <div class="card-footer p-2 has-text-right">
                <button class="button has-background-danger-dark has-text-white" on:click={confirm()}
                    >確定</button
                >
                <button
                    class="button ml-2 has-background-link-light"
                    on:click={()=>{cancel()}}>取消</button
                >
            </div>
        </div>
    </div>
</div>

<style>
    @import "../../javascript/bundle.css";
</style>