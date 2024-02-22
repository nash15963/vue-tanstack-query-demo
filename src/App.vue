<script setup>
import { ref } from 'vue' ;
import { useQuery } from '@tanstack/vue-query'
// https://tanstack.com/query/latest/docs/framework/vue/reference/useQuery

const mockEncytion = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (Math.random() > 0.5) {
        resolve("加密成功");
      } else {
        console.log("加密失败")
        reject(new Error("加密失败"));
      }
    }, 1000);
  });
};
const fetcher = async (id) =>
  await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
  .then((response) => response.json())
  .then((data) => {
    console.log(data) ;
    return data ;
  })


const fetchData = async (id) => {
  try {
    await mockEncytion(); 
    return await fetcher(id); 
  } catch (error) {
    throw error;
  }
};


const { isError, isFetching, data, error,  } = useQuery({
    queryKey: ['post', 1],
    retry : 1,
    networkMode: 'online', //In this mode, Queries and Mutations will not fire unless you have network connection. 
    queryFn: () => fetchData(1),
  })


</script>

<template>
  <h1>資料溝通狀態</h1>
  <p>Fetch 狀態 ：{{ isFetching ? "資料更新中" : "更新結束" }}</p>
  <p>是否錯誤 ：{{ isError ? "是" : "否" }}</p>
  <p>資料 ：{{ data }}</p>
  <p>錯誤 ：{{ error?.message }}</p>

</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
