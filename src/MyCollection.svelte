<script>
    import CreateCollection from './CreateCollection.svelte';
    import {signInMetamask, SessionData} from './Session'
    import {axios} from './Singleton'
    import CollectionCard from './CollectionCard.svelte'
    import {onMount} from 'svelte'
    let datas = [
];

    async function OnCreateCollectionButtonClick()
    {
        if(!$SessionData)
        {
            alert("Please, sign in.");
            return;
        }
        let contenttag = document.getElementsByTagName('content')[0];
        new CreateCollection({target: contenttag, hydrate: true});
    }

    async function getData()
    {
        let result = await $axios.get("/collections/mycollections");
        datas = result.data;
    }
    onMount(getData);
</script>

<div class="explorediv">
    <h2>My Collections</h2>
    <h6>Create, curate, and manage collections of unique NFTs to share and sell.</h6>
    <button on:click={OnCreateCollectionButtonClick}>Create a Collection</button>
    <div class="collectionsdiv">
    {#each datas as data}
    <!--
        <div class="collectiondiv">
            <img class="collectionimg" src={data.img}/>
            <img class="collectionauthorimg" src={data.profileimg}>
            <div class="collectiondescriptiondiv">
                <h6>{data.title}</h6>
                <h7>by {data.author.name}</h7>
                <p>{data.description}</p>
                <p>{data.count} items</p>
            </div>
        </div>
    -->
        <CollectionCard collectionid={{id:data.id}}></CollectionCard>
    {/each}
    </div>
</div>

<style>
.collectionsdiv
{
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.collectiondiv
{
    display: inline-block;
    height: 20em;
    width: 23em;
    border: 1px solid grey;
    border-radius: 1em;
    margin: 1em;
    overflow: hidden;
}

.collectionimg
{
  display: block;
  width: 23em;
  height: 50%;
}

.collectionauthorimg
{
    display: inline;
    width: 3em;
    height: 3em;
    text-align: center;
    position: relative;
    top: -2.9em;
    margin:0;
    border-radius:2em;
}

.explorediv
{
    width: 95vw;
    text-align:center;
    position: relative;
    left:2.5vw;
}

.collectiondescriptiondiv
{
    position: relative;
    top: -4.8em;
}

h7
{
    margin: 0;
}
</style>