<script lang="ts">
  let theInput;
  let url = new URL(window.location.toString())
  let user:string
  user = url.searchParams.get("name") || "MiftikCZ"
  let projekty = {"...": "https://github.com/"+user};
  (async () => { 
    projekty = await (await fetch(`https://api.github.com/users/${user}/repos`)).json()
  })()
  let inputval:string
  let filterBy:string = "updated_at" 
</script>

<main>
  <span class="tt">
    <span class="wrap">
      <span class="title">
        Git Browse 
      </span>
    </span>

    <span class="wrap">
      <span class="title">
        <span class="hint2">{`[${projekty?.length}]`}</span>
      </span>
    </span>

    <span class="wrap">
      <span class="title">
        <span class="hint">{`{${user}}`}</span>
      </span>
    </span>
  </span> 
  <div class="main">

  <input class="input" bind:value={inputval} placeholder="Search something..." type="text" bind:this={theInput}>

  <div class="projekty">
  {#each Object.values(projekty).sort((a,b) => Date.parse(b?.[filterBy]) - Date.parse(a?.[filterBy])) as title}
    {#if !inputval || title?.["name"].includes(inputval)}
    <a class="projekt" href={title?.["html_url"]} target="_blank">
      {@html //@ts-ignore 
        title["name"]?.split(inputval).join("<span class='highlight'>" + inputval + "</span>")}  
    </a>
    {/if}
  {/each}
  </div>
  </div>
</main>

<style>
  .main {
    max-width: 600px;
    display: flex;
    gap: 2em;
    height: 100%;
    width: 100%;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  main {
  color: #eceff4;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100%;
    width: 100%;
    display: flex;
    gap: 2em;
  }
  .tt {

    gap: 0.5em;
    display: flex;
    flex-direction: row;
  }
  .wrap {
    display: flex;
    flex-direction: row;
    gap: 0.5em;
    height: 100%;
    border: #eceff412 1px solid;
    box-shadow: #2e3440 0 0 20px;
    background: #2e3440;
  }
  .title {
    border-radius: 0.25em;
  font-weight: bold;
  font-size: 1.75em;
  padding: 0 0.5em;
  background: linear-gradient(45deg,
      hsl(calc(var(--hue) - 30), 50%, 53%),
      hsl(calc(var(--hue) + 30), 50%, 53%));
  /* text-shadow: #fff1 3px 3px 3px; */
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.title .hint {
  background: linear-gradient(45deg, #bf616a,#d08770, #ebcb8b);
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.title .hint2 {
  background: linear-gradient(45deg, #8fbcbb,#81a1c1, #5e81ac);
  background-clip: text;
  -webkit-text-fill-color: transparent;
}


.input {
  padding: 0.5em;
  color: #eceff4;
  font-size: 1em;
  width:100%;
  border: none;
  background: #2e3440;
  /* text-shadow: #fff1 3px 3px 3px; */
  border-image: linear-gradient(45deg, #bf616a,#d08770);
  border-width:  0px;
  border-style: solid;
  border-image-slice: 1;
  border-top: none;
  /* border-left: none; */
  border-right: none;
  border-bottom: none;
  transition: 150ms;
}

.input:focus-within {
  outline: none;
}

.input:focus {
  border-width:  3px;
}

.projekty {
  height: 100%;
  padding: 0.25em;
  max-height:50vh;
  overflow: auto;
  display: flex;
  gap: 0.25em;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.projekt {
  color: #eceff4;
  text-decoration: none;
  padding: 0.25em;
  border-left: #d08770 2px solid;
  text-align: center;
  width: 100%;
  background: #2e3440;
  transition: 200ms;
}

.projekt:hover{
  border-left-color: #5e81ac;
  background: #434c5e;
}

:global(.highlight) {
  color: #5e81ac;
  border-bottom: #5e81ac 2px solid;
  /* background: #5e81ac; */
}
</style>