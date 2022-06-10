<svelte:options tag="place-list" />

<script>
    import { get_current_component } from "svelte/internal";
    export let places = [];

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

    const component = get_current_component();

    let del = (preid, placename) => {
        component.dispatchEvent &&
            component.dispatchEvent(
                new CustomEvent("del", { detail: { preid, placename } })
            );
    };
</script>

{#if places.length > 0}
    <table class="table is-striped is-hoverable">
        <thead>
            <th />
            <th>編號</th>
            <th>場地名稱</th>
            <th>借用事由/日期</th>
        </thead>
        <tbody>
            {#each places as p}
                <tr>
                    <td>
                        <button
                            class="button is-danger is-light"
                            on:click={del(p.Preid, p.Placename)}
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
{:else}
    <div class="box is-size-3">無借用場地資料</div>
{/if}

<style>
    @import "../../javascript/bundle.css";
</style>
