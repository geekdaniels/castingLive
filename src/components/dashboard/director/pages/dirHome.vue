<template>
    <div>
        <loader v-if="loading" />
        <div class="card m-b-30">
            <div class="card-body">
                <p class="cv1">
                    <b class="col-ppd">Recent Projects</b>

                    <router-link class="mdb float-right" v-bind:to="'/director/projects'">View More</router-link>
                </p>
                <div style="margin-top: 12px;" class="alert" v-bind:class="{ success: status, danger: !status }" v-if="error">{{
                    error }}</div>
                <div class="cv" v-for="recent in recentProject.data.list" style="position: relative;">
                    <div class="col-md-12 mt-2">
                        <div>
                            <div>
                                <p class="projectStat">FREE</p>
                            </div>
                            <div class="row">
                                <div class="col-md-4" style="overflow-y: hidden;">
                                    <img class="wdd1 float-left mr-4 rounded" :src="recent.image">
                                </div>
                                <div class="col-md-8">
                                    <h2>
                                        <router-link class="col-bb" v-bind:to="'/project/details/'+recent.id">{{recent.title}}</router-link>

                                        <div class="dropdown">
                                            <button class="float-right btn btn-ppd btn-sm dropdown-toggle" type="button"
                                                data-toggle="dropdown" style="bottom: 40px;">
                                                Action
                                                <span class="caret"></span>
                                            </button>
                                            <div id="pos">
                                                <ul class="dropdown-menu">
                                                    <li>
                                                        <router-link class="dropdown-item" v-bind:to="'/project/edit/'+recent.id">Edit</router-link>
                                                    </li>
                                                    <li>

                                                        <router-link class="dropdown-item" v-bind:to="'/project/manageRole/'+recent.id">Manage
                                                            Roles</router-link>
                                                    </li>
                                                    <li>
                                                        <a style="cursor: pointer;" class="dropdown-item" v-on:click="deleteProject(recent.id)">Delete</a>
                                                    </li>
                                                </ul>
                                            </div>
                                        </div>
                                    </h2>

                                    <p><b class="col-ppd">Description</b>: {{recent.description}}</p>
                                    <p><b class="col-ppd">Total Applicants</b>: {{recent.applicants}}</p>
                                    <p><b class="col-ppd">Total Roles</b>: {{recent.roles_created}}</p>
                                    <div class="actions">
                                        <button data-toggle="tooltip" data-placement="bottom" title="Tooltip on bottom" class="btn btn-sm btn-pink text-white">Edit Project</button>
                                        <button class="btn btn-sm btn-pink text-white">Manage Roles</button>
                                        <button class="btn btn-sm btn-danger text-white">Delete Project</button>
                                    </div>
    
                                    
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import Loader from '../template/loader';
    export default {
        name: 'Home',
        data() {
            return {
                loading: true,
                recentProject: '',
                qualifiedData: '',
                token: '',
                error: '',
                deleteData: '',
                siteUrl: 'https://api.cast.i.ng/',
            };
        },
        components: {
            loader: Loader,
        },
        mounted() {

            this.token = JSON.parse(localStorage.getItem('token'));
            console.log(this.token);

            var config = {
                headers: {
                    'Access-Control-Allow-Origin': '*'
                },
            };

            let userID = JSON.parse(localStorage.getItem('token'));

            axios({
                method: 'GET',
                url: 'https://api.cast.i.ng/director/recentproject/' + userID + '?limit=2',
                config,
            }).then(
                result => {
                    this.recentProject = result;
                },
                error => {
                    console.log('API CALL FAILED');
                    console.error(error);
                }
            );

            this.loading = true;
            axios.get('https://jsonplaceholder.typicode.com/todos/1').then(
                response => {
                    this.loading = false;
                    console.log('Page Changes');
                },
                error => {
                    this.loading = false;
                    console.log('Page Error');
                }
            );
        },
        methods: {
            deleteProject(projectID) {
                // confirm('Are you sure?');
                var config = {
                    headers: {
                        'Access-Control-Allow-Origin': '*'
                    },
                };

                axios({
                    method: 'GET',
                    url: 'https://api.cast.i.ng/delete/project/' + projectID,
                    config
                }).then(
                    result => {
                        this.loading = false;
                        this.deleteData = result;
                        this.error = result.data.status_msg;

                        location.reload();

                        // this.$router.replace(this.$route.query.redirect || '/dashboard/profile');
                    },
                    error => {
                        this.loading = false;
                        console.log('API CALL FAILED');
                        // this.$router.replace(this.$route.query.redirect || '/dashboard/profile');
                        console.error(error);
                    }
                );
            },
        },
    };
</script>

<style>
    .form-loader {
        width: 22px;
    }

    .success {
        color: #155724;
        background-color: #d4edda;
        border-color: #c3e6cb;
    }

    .danger {
        color: #721c24;
        background-color: #f8d7da;
        border-color: #f5c6cb;
    }

    .projectStat{
        color: white;
    background: #e6077c;
    display: inline-block;
    font-weight: bold;
    padding: 2px 15px;
    position: absolute;
    z-index: 99;
    top: 0;
    font-size: 12px;
    border: 1px dashed;
}
</style>