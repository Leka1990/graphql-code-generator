<script setup lang="ts">
import { useClient, useQuery } from 'villus';
import { graphql } from './gql/gql';
import FilmItem from './components/FilmItem.vue';
import { computed } from 'vue';

useClient({
  url: 'https://swapi-graphql.netlify.app/.netlify/functions/index',
});

const { data } = useQuery({
  query: graphql(/* GraphQL */ `
    query allFilmsWithVariablesQuery($first: Int!) {
      allFilms(first: $first) {
        edges {
          node {
            ...FilmItem
          }
        }
      }
    }
  `),
  variables: { first: 10 },
});
const films = computed(() => data.value?.allFilms?.edges?.map(e => e?.node!));
</script>

<template>
  <ul>
    <li v-for="film of films"><FilmItem :film="film" /></li>
  </ul>
</template>
