<template>
  <v-container>
    <v-row class="text-center">
      <!-- Ejemplo de una mutacion (post, patch, update, delete) -->
      <ApolloMutation :mutation="require('@/graphql/register.gql')" :variables="{email, password}" @done="onDone">
      <template v-slot="{mutate}">
        <div>
          <v-btn @click="mutate">Soy una mutacion</v-btn>
        </div>
      </template>
      </ApolloMutation> 
      <!-- <ApolloQuery :query="gql => gql`
        query {
          characters (page: 2) {
             results {
              name
              id
            }
          }
        }
        `"> -->
        <v-text-field type="number" min="0" v-model.number="page"></v-text-field>
        <ApolloQuery :query="require('@/graphql/characters.gql')" :variables="{page}">
        <template v-slot="{result: {loading, error, data}}">
          <div v-if="data">
             <v-col cols="12" v-for="characters in data.characters.results" :key="characters.id">
              <h1>{{characters.name}}</h1>
              <h1>{{characters.id}}</h1>
            </v-col>
          </div>
        </template>

      </ApolloQuery>
      <!-- <v-col cols="12" v-for="characters in characters.results" :key="characters.id">
        <h1>{{characters.name}}</h1>
      </v-col> -->
    </v-row>
  </v-container>
</template>

<script>
import gql from 'graphql-tag'
  export default {
    name: 'HelloWorld',
    props: {
    },
    apollo: {
        characters: gql`
        query {
          characters (page: 2) {
             results {
              name
              id
            }
          }
        }
        `
      },
    data: () => ({
    page:1,
    email: 'shenick@gmail.com',
    password:'1234'
    }),
    created() {
      
    },
    methods: {
      onDone (returnSomething) {
        //las mutaciones si retornan algo se reciben como argumento
        console.log("funciona", returnSomething)
      }
    },
  }
</script>
