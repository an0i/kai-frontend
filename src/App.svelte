<script>
  let id = ''
  let password = ''
  let content = ''
  let api = 'https://api.kai.anqi.eu.org/api'
  let isLoading = false

  function post(action) {
    const payload = {
      id: action === 'create' ? undefined : id,
      password,
      content: action === 'pull' || action === 'destroy' ? undefined : content
    }

    isLoading = true
    fetch(`${api}/instance/${action}`, {
      method: 'POST',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(payload)
    })
    .then(res=>res.json())
    .then(data=>{
      if (data.success) {
        if (action==='create') {
          id = data.data.id
        } else if (action==='save') {

        } else if (action==='pull') {
          content = data.data.content
        } else if (action==='destroy') {
          id = ''
          password = ''
          content = ''
        }
      } else {
        throw new Error(data.message)
      }
    })
    .catch(e=>{
      alert(e.message)
    })
    .finally(()=>{
      isLoading = false
    })
  }
</script>

<div class="container mx-auto flex h-screen flex-col gap-4 p-4">
  <div>
    <h1 class="text-4xl">Kai</h1>
  </div>
  <div>
    <div class="flex-wrap gap-2 sm:flex">
      <button on:click={()=>{post('create')}} disabled={isLoading===true} class="disabled:bg-gray-100 rounded border border-gray-300 px-4 py-1.5 hover:bg-gray-50 hover:shadow hover:active:bg-gray-100" type="button">创建</button>
      <button on:click={()=>{post('save')}} disabled={isLoading===true} class="disabled:bg-gray-100 rounded border border-gray-300 px-4 py-1.5 hover:bg-gray-50 hover:shadow hover:active:bg-gray-100" type="button">保存</button>
      <button on:click={()=>{post('pull')}} disabled={isLoading===true} class="disabled:bg-gray-100 rounded border border-gray-300 px-4 py-1.5 hover:bg-gray-50 hover:shadow hover:active:bg-gray-100" type="button">拉取</button>
      <button on:click={()=>{post('destroy')}} disabled={isLoading===true} class="disabled:bg-gray-100 ml-auto rounded border border-gray-300 px-4 py-1.5 text-red-600 hover:bg-gray-50 hover:shadow hover:active:bg-gray-100" type="button">销毁</button>
    </div>
  </div>
  <div>
    <div class="flex-wrap gap-2 sm:flex">
      <input bind:value={id} disabled={isLoading===true} placeholder="实例" class="mb-1 w-full rounded border border-gray-300 p-3 hover:shadow focus:outline-gray-600 sm:mb-0 sm:w-fit" type="text" />
      <input bind:value={password} disabled={isLoading===true} placeholder="密码" class="w-full flex-grow rounded border border-gray-300 p-3 hover:shadow focus:outline-gray-600 sm:w-fit" type="text" />
    </div>
  </div>
  <div>
    <textarea bind:value={content} disabled={isLoading===true} placeholder="内容" class="h-48 w-full rounded border border-gray-300 p-3 align-middle hover:shadow focus:outline-gray-600" rows="6" />
  </div>
  <div>
    <input bind:value={api} disabled={isLoading===true} placeholder="API" class="w-full rounded border border-gray-300 p-3 hover:shadow focus:outline-gray-600" type="text" />
  </div>
  <div>
    <p class="w-fit rounded bg-gray-100 px-3 py-2 text-sky-950">密码仅在创建时指定，实例无保存或拉取操作10分钟自动销毁</p>
  </div>
  {#if isLoading}
    <div>
      <div class="flex justify-center">
        <div class="px-3 py-2 bg-gray-900 text-white rounded fixed top-6">
          加载中...
        </div>
      </div>
    </div>
  {/if}
</div>
