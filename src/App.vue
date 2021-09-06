<template>
  <div id="app">
    <nav class="navbar navbar-expand">
      <a href="/" class="navbar-brand">Higashinomirai</a>
      <div class="navbar-nav mr-auto">
        <li class="nav-item">
          <router-link to="/home" class="nav-link">
            Home
          </router-link>
        </li>
        <li v-if="showAdminBoard" class="nav-item">
          <router-link to="/admin" class="nav-link">Admin Board</router-link>
        </li>
        <li v-if="showModeratorBoard" class="nav-item">
          <router-link to="/mod" class="nav-link">Moderator Board</router-link>
        </li>
        <li class="nav-item">
          <router-link v-if="currentUser" to="/user" class="nav-link">User</router-link>
        </li>
        <li class="nav-item">
          <router-link v-if="currentUser" to="/chat" class="nav-link">ChatList</router-link>
        </li>
      </div>

      <div v-if="!currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link to="/register" class="nav-link">
            Sign Up
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/login" class="nav-link">
            Login
          </router-link>
        </li>
      </div>

      <div v-if="currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link to="/profile" class="nav-link">
            <font-awesome-icon icon="user" />
            {{ currentUser.username }}
          </router-link>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click.prevent="logOut">
            <font-awesome-icon icon="sign-out-alt" /> LogOut
          </a>
        </li>
      </div>
    </nav> -->

    <v-card
      class="mx-auto overflow-hidden"
      height="400"
    >
      <v-app-bar
        color="deep-purple"
        
      >
        <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>

        <v-toolbar-title>Admin Page</v-toolbar-title>
      </v-app-bar>

      <v-navigation-drawer
        v-model="drawer"
        absolute
        temporary
      >
        <v-list
          nav
          dense
        >
          <v-list-item-group
            v-model="group"
            active-class="deep-purple--text text--accent-4"
          >
            <v-list-item>
              <v-list-item-icon>
                <router-link to="/home" class="nav-link">
                  <v-icon>mdi-home</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Home</v-list-item-title>
            </v-list-item>

            
            <router-link to="/admin" class="nav-link" v-if="showModeratorBoard" >
              <v-list-item>
                <v-list-item-icon>
                    <v-icon>mdi-admin</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Admin</v-list-item-title>
              </v-list-item>
            </router-link>

            
            <v-list-item v-if="showModeratorBoard">
              <v-list-item-icon>
                <router-link to="/mod" class="nav-link">
                  <v-icon>mdi-mod</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Moderator Board</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="currentUser">
              <v-list-item-icon>
                <router-link to="/user" class="nav-link">
                  <v-icon>mdi-user</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>User</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="currentUser">
              <v-list-item-icon>
                <router-link to="/chat" class="nav-link">
                  <v-icon>mdi-chat</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>ChatList</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="!currentUser">
              <v-list-item-icon>
                <router-link to="/register" class="nav-link">
                  <v-icon>mdi-sign-up</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Sign Up</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="!currentUser">
              <v-list-item-icon>
                <router-link to="/login" class="nav-link">
                  <v-icon>mdi-login</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Log In</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="currentUser">
              <v-list-item-icon>
                <router-link to="/profile" class="nav-link">
                  <v-icon>mdi-profile</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Profile</v-list-item-title>
            </v-list-item>

            <v-list-item v-if="currentUser">
              <v-list-item-icon>
                <router-link class="nav-link" @click.prevent="logOut">
                  <v-icon>mdi-sign-out</v-icon>
                </router-link>
              </v-list-item-icon>
              <v-list-item-title>Log Out</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>
      


    </v-card>
      <div class="container">
        <div>
          <template>
            <v-simple-table >
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">
                      Age
                    </th>
                    <th class="text-left">
                      First_name
                    </th>
                    <th class="text-left">
                      Last_name
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="item in listSample"
                    :key="item.first_name"
                  >
                    <td>{{ item.age }}</td>
                    <td>{{ item.first_name }}</td>
                    <td>{{ item.last_name }}</td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>


            <router-view />


            <v-btn
              :key="activeFab.icon"
              :color="activeFab.color"
              fab
              large
              
              bottom
              right
              class="v-btn--fix"
            >
              <v-icon>{{ activeFab.icon }}</v-icon>
            </v-btn>
            <v-footer
              
              padless
            >
              <v-card
                flat
                tile
                class="indigo lighten-1 white--text text-center"
              >
                <v-card-text>
                  <v-btn
                    v-for="icon in icons"
                    :key="icon"
                    class="mx-4 white--text"
                    icon
                  >
                    <v-icon size="24px">
                      {{ icon }}
                    </v-icon>
                  </v-btn>
                </v-card-text>

                <v-card-text class="white--text pt-0">
                  Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet. Mauris cursus commodo interdum. Praesent ut risus eget metus luctus accumsan id ultrices nunc. Sed at orci sed massa consectetur dignissim a sit amet dui. Duis commodo vitae velit et faucibus. Morbi vehicula lacinia malesuada. Nulla placerat augue vel ipsum ultrices, cursus iaculis dui sollicitudin. Vestibulum eu ipsum vel diam elementum tempor vel ut orci. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.
                </v-card-text>

                <v-divider></v-divider>

                <v-card-text class="white--text">
                  {{ new Date().getFullYear() }} — <strong>Admin Page</strong>
                </v-card-text>
              </v-card>
            </v-footer>



          </template>
        </div>
        <router-view />
      </div>




  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Name"
      required
    ></v-text-field>

    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="E-mail"
      required
    ></v-text-field>

    <v-select
      v-model="select"
      :items="items"
      :rules="[v => !!v || 'Item is required']"
      label="Item"
      required
    ></v-select>

    <v-checkbox
      v-model="checkbox"
      :rules="[v => !!v || 'You must agree to continue!']"
      label="Do you agree?"
      required
    ></v-checkbox>

    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      Validate
    </v-btn>

    <v-btn
      color="error"
      class="mr-4"
      @click="reset"
    >
      Reset Form
    </v-btn>

    <v-btn
      color="warning"
      @click="resetValidation"
    >
      Reset Validation
    </v-btn>
  </v-form>


    <v-card height="400px">
      <v-footer
        v-bind="localAttrs"
        :padless="padless"
      >
        <v-card
          flat
          tile
          width="100%"
          class="red lighten-1 text-center"
        >
          <v-card-text>
            <v-btn
              v-for="icon in icons"
              :key="icon"
              class="mx-4"
              icon
            >
              <v-icon size="24px">
                {{ icon }}
              </v-icon>
            </v-btn>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-text class="white--text">
            {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
          </v-card-text>
        </v-card>
      </v-footer>

      <v-row
        align="center"
        justify="center"
        class="ma-12"
      >
        <v-col
          cols="12"
          md="8"
        >
          <v-select
            v-model="variant"
            :items="items"
            clearable
            label="Variant"
          ></v-select>

          <v-checkbox
            v-model="padless"
            hide-details
            label="Padless"
          ></v-checkbox>
        </v-col>
      </v-row>
    </v-card>
  </div>



  
</template>

<script>
export default {
  data() {
    return {
      listSample: [
        { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
        { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
        { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
        { age: 38, first_name: 'Jami', last_name: 'Carney' }
      ],
      drawer: false,
      group: null,
      icons: [
      'mdi-facebook',
      'mdi-twitter',
      'mdi-linkedin',
      'mdi-instagram',
    ],
    fab: false,
    hidden: false,
    }
  },
  computed: {
    activeFab () {
      return { color: 'success', icon: 'mdi-chevron-up' }
    },
    currentUser() {
      return this.$store.state.auth.user;
    },
    showAdminBoard() {
      if (this.currentUser && this.currentUser['roles']) {
        return this.currentUser['roles'].includes('ROLE_ADMIN');
      }
      return false;
    },
    showModeratorBoard() {
      if (this.currentUser && this.currentUser['roles']) {
        return this.currentUser['roles'].includes('ROLE_MODERATOR');
      }
      return false;
    },
    localAttrs() {
      const attrs = {}

      if (this.variant === 'default') {
        attrs.absolute = false
        attrs.fixed = false
      } else {
        attrs[this.variant] = true
      }
      return attrs
    },
  },
  methods: {
    logOut() {
      this.$store.dispatch('auth/logout');
      this.$router.push('/login');
    }
  }
};
</script>
<style>
  #lateral .v-btn--fix {
    right: 16px;
    bottom: 16px;
    position: absolute;
    margin: 0 0 16px 16px;
  }
</style>