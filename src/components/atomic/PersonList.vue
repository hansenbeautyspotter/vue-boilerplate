<template lang="html">
  <div class="PersonListWrapper">
    <div class="field">
      <p class="control">
        <input class="input" type="text" placeholder="Search Name" v-model="PersonListSearch">
      </p>
    </div>
    <ul class="personList">
      <li v-for="person in filteredItems">
        <div class="media">
          <figure class="media-left">
            <p class="image is-64x64">
              <img src="http://bulma.io/images/placeholders/128x128.png">
            </p>
          </figure>
          <div class="media-content">
            <div class="content">
              <strong>{{ person.id }}. {{ person.name }}</strong> <small>@{{person.username}}</small>
              <p>
                Lorem ipsum dolor sit amet, consectetur adipiscing elit.
              </p>
            </div>
            <nav class="level is-mobile">
              <div class="level-left">
                <a class="level-item">
                  <span class="icon is-small"><i class="fa fa-chevron-up"></i></span>
                </a>
                <a class="level-item">
                  <span class="icon is-small"><i class="fa fa-chevron-down"></i></span>
                </a>
              </div>
            </nav>
          </div>
          <div class="media-right">
            <i class="fa fa-map-marker marker-action" @click="markerAction(person.id, person.address.geo, $event)"></i>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios';
  import eventHub from '@/util/EventHub'

  export default {
    name: "personList",
    data() {
      return {
        PersonListSearch: '',
        data: {
          persons: []
        }
      }
    },
    created() {
      var mockAPI = 'https://jsonplaceholder.typicode.com/users';

      axios.get(mockAPI).then(res => {
        this.data.persons = res.data;
      });
    },
    methods: {
      markerAction: (personId, geoData, event) => {
        geoData = {
          lat: parseFloat(geoData.lat),
          lng: parseFloat(geoData.lng)
        }
        eventHub.$emit('maps-create-marker', {markerId: personId, geoData: geoData});
      }
    },
    computed: {
      filteredItems() {
        return this.data.persons.filter(item => {
          return item.name.toLowerCase().indexOf(this.PersonListSearch.toLowerCase()) > -1
        });
      }
    }
  }
</script>

<style lang="scss">
  .PersonListWrapper {
    height: 100%;
    overflow: hidden;

    .field {
      padding: 16px 16px 0;
    }
  }

  .personList {
    list-style: none;
    margin: 0;
    padding: 16px;
    max-height: calc(100% - 40px);
    overflow: auto;

    li {
      margin-bottom: 20px;

      .marker-action {
        cursor: pointer;
      }
    }
  }
</style>
