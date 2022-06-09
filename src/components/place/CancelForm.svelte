<svelte:options tag="cancel-form" />

<script>
    import { onMount } from "svelte";
    import ConfirmModal from "../public/ConfirmModal.svelte";

    let myPlaces = [];

    let delObject = {
        preid: 0,
        placename: "",
    };

    let delModal = false;

    let delConfirm = (id, name) => {
        delObject.preid = id;
        delObject.placename = name;
        delModal = true;
    };

    let cancel = () => {
        debugger;
    };

    $: delContent = `確定要移除<em>編號:${delObject.preid},場地名稱:${delObject.placename}</em>的借用嗎?<br/>再次提醒您,一旦取消借用後,即無法再復原,若有任何問題,請洽場地借用單位.`;

    onMount(() => {
        fetch("./service/PreAgreement.ashx?cmd=query", { method: "GET" })
            .then((res) => res.json())
            .then((res) => {
                myPlaces = [...myPlaces, ...res];
            });
       
    });

    const toDate = (d) => {
        let t = new Date(parseInt(d.replace("/Date(", "").replace(")/", "")));
        return `${t.getFullYear().toString().padStart(2, "0")}/${t
            .getMonth()
            .toString()
            .padStart(2, "0")}/${t.getDate().toString().padStart(2, "0")} ${t
            .getHours()
            .toString()
            .padStart(2, "0")}:${t.getMinutes().toString().padStart(2, "0")}`;
    };
</script>

<confirm-modal
    modal={delModal}
    content={delContent}
    title="移除借用場地"
    on:confirm={() => {
        delModal = false;
    }}
    on:cancel={()=>{delModal=false}}
/>

<table class="table is-striped is-hoverable">
    <thead>
        <th />
        <th>編號</th>
        <th>場地名稱</th>
        <th>借用事由/日期</th>        
    </thead>
    <tbody>
        {#each myPlaces as p}
            <tr>
                <td>
                    <button
                        class="button is-danger is-light"
                        on:click={delConfirm(p.Preid, p.Placename)}
                        >移除借用</button
                    >
                </td>
                <td>
                    {p.Preid}
                </td>
                <td>
                    {p.Placename}
                </td>
                <td>
                    <p>
                        {p.reason}
                    </p>
                    <div>
                        {toDate(p.start_date)} ~ {toDate(p.end_date)}
                    </div>
                    
                </td>              
            </tr>
        {/each}
    </tbody>
</table>

<style>
    @import "../../javascript/bundle.css";
</style>
