<!-- eslint-disable vue/require-v-for-key -->
<template>
    <div id="list-users">
      <h2>Twubric</h2>
      <div id="filter">
        <h6>Sort By</h6>
        <div class="btn-group" role="group" >
					<button
						type="button"
						class="btn"
						v-for="(filterMethod, index) in filterMethods"
						:key="filterMethod.code"
						:class="[
							selectedMethod === filterMethod.code
								? 'btn-success disabled-cursor border-0 shadow-sm'
								: 'btn-default border-0 shadow-sm',
							index === 0 ? 'curve-left' : 'curve-right'
						]"
						:style="['border-radius: 0.35rem 0 0 0.35rem !important']"
						@click="sortList(filterMethod.code,filterMethod.method)">
                {{ filterMethod.name }}
                <span v-if="sortedbyASC && selectedMethod === filterMethod.code" class="material-symbols-outlined">
                  arrow_downward
                </span>
                <span v-else-if="sortedbyASC === false && selectedMethod === filterMethod.code" class="material-symbols-outlined">
                  arrow_upward
                </span>
					</button>
				</div>
        <hr/>
        <h6>
          Joined Twitter between
        </h6>
        <div class="row" style="display: flex;
    justify-content: center">
          <div class="col-lg-3 col-md-3 col-sm-3 col-xs-12">
            <div class="flatpickr">
              <label for="startTime">
               Start Date/Time :
              </label>
              <br />
              <flat-pickr
                v-model="filters.startDate"
                :config="flatpickrDateTimeConfig"
                id="startTime"
                class="form-control" />
              <br />
            </div>
          </div>
          <div class="col-lg-3 col-md-3 col-sm-3 col-xs-12">
            <div class="flatpickr">
              <label for="endTime" > End Date/Time : </label>
              <br />
              <flat-pickr
                v-model="filters.endDate"
                :config="flatpickrDateTimeConfig"
                id="endTime"
                class="form-control" />
            </div>
          </div>
        </div>

      </div>
        <div id="user-list" class="row">
					<UserCard
						v-for="(user, index) in filteredList"
						class="col-xl-4 col-lg-4 col-md-6 col-sm-12 col-xs-12"
						:key="'user-' + index"
						:user-id="user.uid"
						:user="user"
            :twubric="user.twubric"
						:slno="index + 1" 
            @remove-user="removeUser($event)"/>
        </div>
    </div>
  </template>
  
  <script>
  import Vue from 'vue';
  import states from "../assets/Users.json";
  import UserCard from './subComponents/UserCard.vue'
  import flatPickr from 'vue-flatpickr-component';
    import 'flatpickr/dist/flatpickr.css';

    import vueMoment from 'vue-moment';

Vue.use(vueMoment);
 


  export default {
    name: "ListUserDetails",
    components: {
      UserCard,
      flatPickr
    },
    data() {
      return {
        filterMethods: [
				{
					name: 'Twubric Score',
					code: 'total',
          method: 'twubric.total'
				},
				{
					name: 'Friends',
					code: 'friends',
          method: 'twubric.friends'
				},
				{
					name: 'Influence',
					code: 'influence',
          method: 'twubric.influence'
				},
				{
					name: 'Chirpiness',
					code: 'chirpiness',
          method: 'twubric.chirpiness'
				}
			],
      selectedMethod: '',
      sortedbyASC: true,
      filteredList: [],
      users: [],
      flatpickrDateTimeConfig: {
				wrap: true, // set wrap to true only when using 'input-group'
				altFormat: 'd-m-Y h:i K',
				altInput: true,
				dateFormat: 'Y-m-d H:i:00',
				enableTime: true
			},
      filters: {
				startDate: null,
				endDate: null
			},
      }

    },
    created(){
      const self = this;
      document.addEventListener("keydown", function(event) {
  if (event.altKey && event.code === "KeyT") {
    self.sortList('total','twubric.total')
    event.preventDefault();
  } else if (event.altKey && event.code === "KeyF") {
    self.sortList('friends','twubric.friends')
    event.preventDefault();
  } else if (event.altKey && event.code === "KeyI") {
    self.sortList('influence','twubric.influence')
    event.preventDefault();
  } if (event.altKey && event.code === "KeyC") {
    self.sortList('chirpiness','twubric.Chirpiness')
    event.preventDefault();
  }
});

    },
    mounted() {
      
    this.users = this.$_.cloneDeep(this.userList)
    this.filteredList = this.$_.cloneDeep(this.userList)
    console.log(this.filteredList);
  },
  watch: {
    filters: {
      handler(){
      if(this.filters.endDate != null && this.filters.startDate != null && this.filters.endDate >= this.filters.startDate ){
        this.filterUsers()
      }
    },
    deep: true
    }
  },
  methods: {
    sortList(sortBy,method) {
      console.log(sortBy);
      if(sortBy != this.selectedMethod) this.sortedbyASC = true;
      this.selectedMethod = sortBy;
      if (this.sortedbyASC) {
        this.filteredList =  this.$_.orderBy(this.users, [method], ['asc']);
        console.log(this.filteredList);
        this.sortedbyASC = false;
      } else {
        this.filteredList =  this.$_.orderBy(this.users, [method], ['desc']);
        console.log(this.filteredList);
        this.sortedbyASC = true;
      }
    },
    filterUsers(){
      this.filteredList = this.$_.filter(this.users, (user) => {
        const date = new Date(user.join_date*1000);
        const startDate = new Date(this.filters.startDate);
        const endDate = new Date(this.filters.endDate);

        if(date >= startDate && date <=   endDate) return user;
      })
    },
    removeUser(userId){
      console.log(userId)
      this.filteredList = this.$_.filter(this.filteredList, (user) => {
        if(user.uid != userId) return user;
      })
      this.users = this.$_.filter(this.users, (user) => {
        if(user.uid != userId) return user;
      })

    }
  },
    computed: {
      userList() {
        return states.users;
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
  </style>
  