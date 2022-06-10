<svelte:options tag="cancel-form" />

<script>
    import { onMount } from "svelte";
    import ConfirmModal from "../public/ConfirmModal.svelte";
    import MessageModal from "../public/MessageModal.svelte";

    import PlaceList from "./PlaceList.svelte";

    let myPlaces = [];

    let delObject = {
        preid: 0,
        placename: "",
    };

    let delModal = false;
    let msgModal = false;
    let message = "";
    let messageType = "success";

    let delConfirm = (obj) => {
        delObject.preid = obj.preid;
        delObject.placename = obj.placename;
        delModal = true;
    };

    let cancelPlace = (reason) => {
        let formdata = new FormData();
        formdata.append("preid", delObject.preid);
        formdata.append("reason", reason);
        fetch("./service/PreAgreement.ashx?cmd=cancel", {
            method: "POST",
            body: formdata,
        })
            .then((res) => res.json())
            .then((res) => {
                if (res.code == 0) {
                    message = "取消場地成功!";
                    messageType = "success";
                    msgModal = true;                    
                    myPlaces = myPlaces.filter(
                        (f) => f.Preid != delObject.preid
                    );                    
                } else if (res.code == 1 || res.code == 2) {
                    message = res.status;
                    messageType = "error";
                    msgModal = true;
                }
            });
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
    on:confirm={(e) => {
        delModal = false;
        cancelPlace(e.detail.reason);
    }}
    on:cancel={() => {
        delModal = false;
    }}
/>

<message-modal
    {message}
    type={messageType}
    modal={msgModal}
    on:close={() => (msgModal = false)}
/>

<place-list
    places={myPlaces}
    on:del={(e) => {
        delConfirm(e.detail);
    }}
/>

<style>
    @import "../../javascript/bundle.css";
</style>
