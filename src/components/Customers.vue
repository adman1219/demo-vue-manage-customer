<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Manage Customers</h1>
    <input class="form-control" placeholder="Enter Last Name" v-model="filterInput"/>
    <br />
    <table class="table table-striped">
      <thead>
      <tr>
        <th>first name</th>
        <th>last name</th>
        <th>email</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
        <tr v-for="customer in filterBy(customers, filterInput)">
          <td>{{customer.firstName}}</td>
          <td>{{customer.lastName}}</td>
          <td>{{customer.email}}</td>
          <td><router-link class="btn btn-default" v-bind:to="'/customer/' + customer.id">View</router-link></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert'
  export default {
    name: 'customers',
    data () {
      return {
        customers: [],
        alert: '',
        filterInput: ''
      }
    },
    methods: {
      fetchCustomers() {
        this.$http.get('http://localhost:8081/api/customers')
          .then(function(res) {
            this.customers = res.body;
          })
      },
      filterBy(list, value) {
        value = value.charAt(0).toLowerCase() + value.slice(1);
        return list.filter(function (customer) {
          return customer.lastName.indexOf(value) > -1;
        });
      }
    },
    created: function () {
      if (this.$route.query.alert) {
        this.alert = this.$route.query.alert;
      }
      this.fetchCustomers();
    },
    updated: function() {
      // this.fetchCustomers();
    },
    components: {Alert}
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
