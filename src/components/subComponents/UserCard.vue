<!-- eslint-disable vue/require-v-for-key -->
<template>
    <div :id="user.uid">
        <div class="card card-default shadow-sm p-2 py-1 px-1" >
            <div class="card-header mb-1 my-1 col-lg-12 col-md-12 col-sm-12 col-12 py-0 d-flex align-items-center justify-content-between">
                <span>
                    <label class="mt-1 mr-2" :for="'select-' + user.uid">
						<b> {{ slno }}.<span>{{user.fullname}}</span> </b>
					</label>
                </span>
                <div class="pull-right">
                    <button
							class="btn btn-danger"
							title="Remove User"
							@click="removeUser()"> Remove 
						</button>
                </div>
            </div>
            <div class="card-body pb-2">
                <div class="row">
					<div class="col-md-7 col-sm-7 col-7 col-xs-8">
						<label class="text-hint">
							Joining Date
						</label>
						<span class="pull-right">:</span>
					</div>
					<div class="col-md-5 col-sm-5 col-5 col-xs-4 bold col-5">
						{{ user.join_date | moment('hh:mm A, DD-MM-YYYY') }}
					</div>
				</div>
                <div class="row">
					<div class="col-md-7 col-sm-7 col-7 col-xs-8">
						<label class="text-hint">
							Friends
						</label>
						<span class="pull-right">:</span>
					</div>
					<div class="col-md-5 col-sm-5 col-5 col-xs-4 bold col-5">
						{{ twubric.friends }}
					</div>
				</div>
                <div class="row">
					<div class="col-md-7 col-sm-7 col-7 col-xs-8">
						<label class="text-hint">
							Influence
						</label>
						<span class="pull-right">:</span>
					</div>
					<div class="col-md-5 col-sm-5 col-5 col-xs-4 bold col-5">
						{{ twubric.influence }}
					</div>
				</div>
                <div class="row">
					<div class="col-md-7 col-sm-7 col-7 col-xs-8">
						<label class="text-hint">
							Chirpiness
						</label>
						<span class="pull-right">:</span>
					</div>
					<div class="col-md-5 col-sm-5 col-5 col-xs-4 bold col-5">
						{{ twubric.chirpiness }}
					</div>
				</div>
            </div>
            <hr class="card-seperator" />
            <div
				class="card-header pt-1 mb-1 my-1 col-lg-12 col-md-12 col-sm-12 col-12 d-flex justify-content-between align-items-center">
				<span class="text-left">
                    <span class="btn btn-curved btn-outline-complete fs-10 mt-1">
                        Total : {{ twubric.total }}
                    </span>
                </span>
            </div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "UserCard",
    computed: {
    },
	props: {
		userId: {
			required: true,
			type: Number,
			default: 0
		},
		slno: {
			type: Number,
			default: 1
		},
		user: {
			type: Object,
			required: true
		},
        twubric:{
			type: Object,
			required: true
		},
	},
    methods: {
        removeUser(){
            this.$emit('remove-user',this.userId);
        }
    }
  };
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  h1,
  h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .actions-btn {
	padding: 0.5rem 0.75rem 0.5rem 0.75rem !important;
	border-radius: 50px !important;
	background-color: transparent !important;
	border-color: rgb(218, 19, 19) !important;
}
  </style>
  
