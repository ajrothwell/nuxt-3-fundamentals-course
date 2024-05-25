<script setup>

const route = useRoute()

const { data, error } = await useFetch(
  // `https://httpbin.org/status/500`
  `https://www.omdbapi.com/?apikey=8e3f600b&i=${route.params.id}`
  , {
  // server: false,
  pick: ["Plot", "Title", "Error", "Poster"],
  key: `/movies/${route.params.id}`,
})

if(error.value) {
  showError({statusCode: 500, statusMessage: "Oh no"})
}

if (data.value.Error === "Incorrect IMDb ID.") {
  showError({statusCode: 404, statusMessage: "Page Not Found" })
}

useHead({
  title: data.value.Title,
  meta: [
    { name: "description", content: data.value.Plot },
    { property: "og:description", content: data.value.Plot },
    { property: "og:image", content: data.value.Poster },
    { name: "twitter:card", content: `summary_large_image` },
  ],
})

// might not be necessary anymore but - whenever you call the
// useAsyncData function, you should also provide it a
// unique key if there's anything variable inside of your
// callback function that can change, making the auto-generated filename
// and line number key inadequate
// const { data } = useAsyncData(
//   `/movies/${route.params.id}`,
//   () => {
//   return $fetch(
//     `https://www.omdbapi.com/?apikey=8e3f600b&i=${route.params.id}`
//   );
// },
// { 
//   // server: true,
//   // lazy: true,
//   pick: ["Plot", "Title"],
//   // transform(data) {
//   //   return {
//   //     Plot: data.Plot,
//   //     Title: data.Title,
//   //   }
//   //   // return data.Title;
//   // }
// }
// );


</script>

<template>
  <div>
    <pre>{{ data }}</pre>
  </div>
</template>