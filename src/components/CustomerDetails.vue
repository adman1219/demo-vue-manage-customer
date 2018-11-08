<template>
  <div class="details container">
    <router-link to="/">Back</router-link>
    <h1 class="page-header">{{customer.firstName}} {{customer.lastName}}
      <span class="pull-right">
        <router-link class="btn btn-primary" v-bind:to="'/edit/'+customer.id">Edit</router-link>
        <button class="btn btn-danger" v-on:click="deleteCustomer(customer.id)">Delete</button>
      </span>
    </h1>
    <ul class="list-group">
      <li class="list-group-item">
        <span class="glyphicon glyphicon-phone" aria-hidden="true"></span>
        {{customer.phone}}
      </li>
      <li class="list-group-item">
        <span class="glyphicon glyphicon-envelope" aria-hidden="true"></span>
        {{customer.email}}
      </li>
    </ul>
    <ul class="list-group">
      <li class="list-group-item">{{customer.address}}</li>
      <li class="list-group-item">{{customer.city}}</li>
      <li class="list-group-item">{{customer.state}}</li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'customerDetails',
    data () {
      return {
        customer: {}
      }
    },
    methods: {
      fetchCustomer(id) {
        this.$http.get('http://localhost:8081/api/customers/' + id)
          .then(function(res) {
            this.customer = res.body;
          })
      },
      deleteCustomer(id) {
        this.$http.delete('http://localhost:8081/api/customers/delete/' + id)
          .then(function(res) {
            this.$router.push({path: '/', query: {alert: 'Customer Deleted'}});
          })
      }
    },
    created: function () {
      this.fetchCustomer(this.$route.params.id);
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
