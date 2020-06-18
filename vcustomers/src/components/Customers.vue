<template>
    <div class="customers container">
        <Alert v-if="alert" v-bind:message="alert"></Alert>
        <h1 class="page-header">用户管理系统</h1>
        
        <input type="text" class="form-control" placeholder="搜索" v-model="filterInput">
        <table class="table table-striped">
            <thead>
                <tr>
                    <td>姓名</td>
                    <td>电话</td>
                    <td>邮箱</td>
                    <td></td>
                </tr>
            </thead>

            <tbody>
                <tr :key="index" v-for="(customer,index) in filterBy(customers,filterInput)">
                    <td>{{customer.name}}</td>
                    <td>{{customer.phone}}</td>
                    <td>{{customer.email}}</td>
                    <td><router-link class="bth btn-default" v-bind:to="'./customer/'+customer.id">详情</router-link></td>
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
            customers:[],
            alert:'',
            filterInput:""
        }
    },
    methods:{
        fetchCustomers(){
            this.$http.get("http://localhost:3000/users")
                .then(function(response){
                    // console.log(response)
                    this.customers=response.body;
                })
        },
        filterBy(customers,value){
            return customers.filter(function (customer) {       //filter过滤器,value为空时，数据全部返回
                return customer.name.match(value)
            })
        }
    },
    created(){
        if(this.$route.query.alert){
            this.alert=this.$route.query.alert;
        }
        this.fetchCustomers()
    },
    updated() {                 //更新事件会不停的调用
        // this.fetchCustomers()       
    },
    components:{
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
