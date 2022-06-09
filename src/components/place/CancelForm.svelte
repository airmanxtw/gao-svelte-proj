<svelte:options tag="cancel-form" />

<script>
    import { onMount } from "svelte";
    import ConfirmModal from "../public/ConfirmModal.svelte";
    import PlaceList from "./PlaceList.svelte";

    let myPlaces = [];

    let delObject = {
        preid: 0,
        placename: "",
    };

    let delModal = false;

    let delConfirm = (obj) => {
        delObject.preid = obj.preid;
        delObject.placename = obj.placename;
        delModal = true;
    };

    $: delContent = `確定要移除<em>編號:${delObject.preid},場地名稱:${delObject.placename}</em>的借用嗎?<br/>再次提醒您,一旦取消借用後,即無法再復原,若有任何問題,請洽場地借用單位.`;

    onMount(() => {
        fetch("./service/PreAgreement.ashx?cmd=query", { method: "GET" })
            .then((res) => res.json())
            .then((res) => {
                myPlaces = [...myPlaces, ...res];
            });
    });
</script>

<confirm-modal
    modal={delModal}
    content={delContent}
    title="移除借用場地"
    on:confirm={() => {
        delModal = false;
    }}
    on:cancel={() => {
        delModal = false;
    }}
/>

<place-list places={myPlaces} on:del={delConfirm} />

<style>
    @import "../../javascript/bundle.css";
</style>
