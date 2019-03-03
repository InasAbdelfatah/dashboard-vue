<template>
  <div class="filters-page">
    
    <vuestic-widget headerText="countries">
      <div class="row">
        <div class="col-md-8"></div>
        <div class="col-md-4">
          <!-- <button style='align:right margin-bottom:5px;;' class="btn btn-primary">add new country</button> -->
          <button class="btn btn-warning" @click="showLargeModal()">
            {{'modal.large' | translate }}
          </button>
          <vuestic-modal :show.sync="show" v-bind:large="true" ref="largeModal"
                   :okText="'modal.confirm' | translate"
                   :cancelText="'modal.cancel' | translate">
      <div slot="title">add new country</div>
      <div>
       <vuestic-widget :headerText="'forms.inputs.title' | translate">
          <form>

            <div class="va-row">
              <div class="flex md8">
                <fieldset>
                  <div class="form-group with-icon-right"
                       :class="{'has-xerror': errors.has('successfulEmail'), 'valid': isSuccessfulEmailValid}">
                    <div class="input-group">
                      <input
                        id="successfulEmail"
                        name="successfulEmail"
                        v-model="successfulEnName"
                        v-validate="'required|email'"
                        required/>
                      <i
                        class="fa fa-exclamation-triangle error-icon icon-right input-icon"></i>
                      <i
                        class="fa fa-check valid-icon icon-right input-icon"></i>
                      <label class="control-label" for="successfulEmail">{{'forms.inputs.emailValidatedSuccess'
                        | translate}} </label><i
                      class="bar"></i>
                      <small v-show="errors.has('successfulEmail')"
                             class="help text-danger">
                        {{ errors.first('successfulEmail') }}
                      </small>
                    </div>
                  </div>
                  <div class="form-group with-icon-right"
                       :class="{'has-error': errors.has('wrongEmail')}">
                    <div class="input-group">
                      <input
                        id="wrongEmail"
                        name="wrongEmail"
                        v-model="wrongEmail"
                        v-validate="'required|email'"
                        required/>
                      <i
                        class="fa fa-exclamation-triangle icon-right input-icon"
                        v-show="errors.has('wrongEmail')"></i>
                      <label class="control-label" for="wrongEmail">{{'forms.inputs.emailValidated'
                        | translate}}</label><i class="bar"></i>
                      <small v-show="errors.has('wrongEmail')"
                             class="help text-danger">{{
                        errors.first('wrongEmail')
                        }}
                      </small>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="input-group">
                      <textarea type="text" id="simple-textarea"
                                required></textarea>
                      <label class="control-label" for="simple-textarea">{{'forms.inputs.textArea'
                        | translate}}</label><i class="bar"></i>
                    </div>
                  </div>
                </fieldset>
              </div>
            </div>

          </form>
        </vuestic-widget>
      </div>
    </vuestic-modal>
          
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <div slot="body">
            <div class="row filters-page__filter-bar-container">
              <filter-bar
                v-model="en_name"
                class="filters-page__filter-bar"
                label="En Name"
              />
              <filter-bar
                v-model="ar_name"
                class="filters-page__filter-bar"
                label=" AR name"
              />
              <vuestic-simple-select
                class="filters-page__filter-bar"
                label="status"
                v-model="status"
                :options="statusList"
              />
            </div>
          </div>
          <div class="filters-page__tags">
            <vuestic-tag
              v-if="en_name"
              :en_name="`Name: ${ en_name }`"
              removable
              @remove="en_name = ''"
            />
            <vuestic-tag
              v-if="ar_name"
              :name="`ar_name: ${ ar_name }`"
              removable
              @remove="ar_name = ''"
            />
            <vuestic-tag
              v-if="status"
              :name="`status: ${ status }`"
              removable
              @remove="status = ''"
            />
            <span
              v-if="this.status || this.en_name || this.ar_name"
              class="filters-page__clear-all-text"
              @click="clearAll"
            >
              Clear all filters
            </span>
          </div>
        </div>
      </div>
    </vuestic-widget>
    <vuestic-widget>
      <div class="table-responsive">
        <table class="table table-striped first-td-padding">
          <thead>
          <tr>
            <td class="filters-page__table-heading">{{ $t('tables.headings.enname') }}</td>
            <td class="filters-page__table-heading">{{ $t('tables.headings.arname') }}</td>
            <td class="filters-page__table-heading">{{ $t('tables.headings.status') }}</td>
            <!-- <td align="right" class="filters-page__table-heading">{{ $t('tables.headings.score') }}</td> -->
            <td>options</td>
          </tr>
          </thead>
          <tbody>
          <tr v-for="item in filteredItems" v-bind:key="item.en_name">
            <td>{{ item.en_name }}</td>
            <td>{{ item.ar_name }}</td>
            <td>{{ item.status }}</td>
            <!-- <td align="right">{{ item.score }}</td> -->
            <td><button @click="deleteCountry(item.id)" class="btn btn-danger">delete</button></td>
          </tr>
          </tbody>
        </table>
      </div>
    </vuestic-widget>
  </div>
</template>

<script>
import VuesticWidget
  from '../../../vuestic-theme/vuestic-components/vuestic-widget/VuesticWidget'
import FilterBar
  from '../../../vuestic-theme/vuestic-components/vuestic-datatable/datatable-components/FilterBar.vue'
import VuesticSimpleSelect
  from '../../../vuestic-theme/vuestic-components/vuestic-simple-select/VuesticSimpleSelect'
import { SpringSpinner } from 'epic-spinners'
import axios from 'axios'
//import { statusList, itemList } from './filtersData'
export default {
  name: 'filters',
  components: {
    VuesticWidget, FilterBar, SpringSpinner, VuesticSimpleSelect
  },
  data () {
    return {
      en_name: '',
      ar_name: '',
      status: '',
      statusList: ['activated', 'deactivated'],
      itemList: [],
      show: true,
    }
  },
  methods: {
    clearAll () {
      this.en_name = ''
      this.ar_name = ''
      this.status = ''
    },
    showLargeModal () {
      this.$refs.largeModal.open()
    },
    showStaticModal () {
      this.$refs.staticModal.open()
    },
    getCountries () {
    axios
      .get('http://test.pos.local/api/setting/countries',{
        params: {
          //ID: 12345
        },
        headers: { 
          'Authorization': 'bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC90ZXN0LnBvcy5sb2NhbFwvYXBpXC9hdXRoXC9sb2dpbiIsImlhdCI6MTU1MTYwMDY0OSwiZXhwIjoxNTUxNjg3MDQ5LCJuYmYiOjE1NTE2MDA2NDksImp0aSI6IlFTZUQwektQWEczc3lhOTMiLCJzdWIiOjEsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEiLCJpZCI6MSwibmFtZSI6InRlc3QiLCJlbWFpbCI6InRlc3RAdGVzdC5jb20iLCJyb2xlcyI6WyJhZG1pbiJdLCJwZXJtaXNzaW9ucyI6W3siaWQiOjEsIm5hbWUiOiJ1c2VyX3VzZXJfY3JlYXRlIiwiZ3VhcmRfbmFtZSI6ImFwaSJ9LHsiaWQiOjIsIm5hbWUiOiJ1c2VyX3VzZXJfZWRpdCIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjozLCJuYW1lIjoidXNlcl91c2VyX2RlbGV0ZSIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjo0LCJuYW1lIjoidXNlcl9yb2xlX2NyZWF0ZSIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjo1LCJuYW1lIjoidXNlcl9yb2xlX2VkaXQiLCJndWFyZF9uYW1lIjoiYXBpIn0seyJpZCI6NiwibmFtZSI6InVzZXJfcm9sZV9kZWxldGUiLCJndWFyZF9uYW1lIjoiYXBpIn1dLCJicmFuY2hlcyI6W119.pgmmJvW0k9DhC_Id_AYCVOSrLEOQyAfI0anUM36BdlI' }
      })
      .then(response => {
        this.itemList = response.data.response
        console.log(this.info);
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
    send(){
        axios.post('/register', this.form)
    },
    deleteCountry(id) {
      if(confirm('Are you sure ?')){
        // fetch(`http://test.pos.local/api/setting/countries/${id}`,{
        //   method:'delete'
        // },
        // )
        // .then(res => res.json())
        // .then(data => {
        //   aletr('company')
        // })
        // .catch(err =>console.log(err));

        axios
      .delete(`http://test.pos.local/api/setting/countries/${id}`,{
        params: {
          //ID: 12345
        },
        headers: { 
          'Authorization': 'bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC90ZXN0LnBvcy5sb2NhbFwvYXBpXC9hdXRoXC9sb2dpbiIsImlhdCI6MTU1MTYwMDY0OSwiZXhwIjoxNTUxNjg3MDQ5LCJuYmYiOjE1NTE2MDA2NDksImp0aSI6IlFTZUQwektQWEczc3lhOTMiLCJzdWIiOjEsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEiLCJpZCI6MSwibmFtZSI6InRlc3QiLCJlbWFpbCI6InRlc3RAdGVzdC5jb20iLCJyb2xlcyI6WyJhZG1pbiJdLCJwZXJtaXNzaW9ucyI6W3siaWQiOjEsIm5hbWUiOiJ1c2VyX3VzZXJfY3JlYXRlIiwiZ3VhcmRfbmFtZSI6ImFwaSJ9LHsiaWQiOjIsIm5hbWUiOiJ1c2VyX3VzZXJfZWRpdCIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjozLCJuYW1lIjoidXNlcl91c2VyX2RlbGV0ZSIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjo0LCJuYW1lIjoidXNlcl9yb2xlX2NyZWF0ZSIsImd1YXJkX25hbWUiOiJhcGkifSx7ImlkIjo1LCJuYW1lIjoidXNlcl9yb2xlX2VkaXQiLCJndWFyZF9uYW1lIjoiYXBpIn0seyJpZCI6NiwibmFtZSI6InVzZXJfcm9sZV9kZWxldGUiLCJndWFyZF9uYW1lIjoiYXBpIn1dLCJicmFuY2hlcyI6W119.pgmmJvW0k9DhC_Id_AYCVOSrLEOQyAfI0anUM36BdlI' }
      })
      .then(response => {
        console.log(response.data.response);
        if(response.data.response == true){
          this.getCountries ();
        }
      })
      .catch(error => {
        console.log(error)
        //this.errored = true
      })
      .finally(() => this.loading = false)
      }
    }
  },
  mounted () {
    this.getCountries ();
  },
  computed: {
    filteredItems () {
      let filteredItems = this.itemList
      if (this.en_name) {
        filteredItems = filteredItems.filter(item => item.en_name.toUpperCase()
          .search(this.en_name.toUpperCase()) !== -1)
      }
      if (this.ar_name) {
        filteredItems = filteredItems.filter(item => item.ar_name.toUpperCase()
          .search(this.ar_name.toUpperCase()) !== -1)
      }
      if (this.status) {
        filteredItems = filteredItems.filter(item => item.status.toUpperCase()
          .search(this.status.toUpperCase()) !== -1)
      }
      return filteredItems
    }
  }
}
</script>

<style lang="scss">
.filters-page {
  &__tags {
    display: flex;
  }
  &__filter-bar {
    width: 300px;
  }
  &__filter-bar-container {
    margin: 0;
    justify-content: space-between;
  }
  &__table-heading {
    color: $vue-green
  }
  &__collapse {
    .vuestic-collapse__body {
      background-color: white;
    }
  }
  &__clear-all-text {
    cursor: pointer;
    color: $vue-green;
    margin-top: 0.3rem;
  }
}

.modals-page {
  .modals-list {
    .btn {
      margin-right: 20px;
      margin-bottom: 25px;
    }
  }
}
</style>
