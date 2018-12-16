<template>
  <v-app>
    <v-container
      grid-list-xl
      text-xs-center
    >
      <v-layout
        row
        wrap
      >
        <v-flex
          xs10
          offset-xs1
        >
          <h1 class="display-3 text-uppercase font-weight-bold">Task</h1>
          <v-form
            ref="form"
            v-model="valid"
            lazy-validation
          >
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
            <v-text-field
              v-model="name"
              :rules="nameRules"
              label="First Name"
              required
            ></v-text-field>
            <v-text-field
              v-model="lastName"
              :rules="nameRules"
              label="Last Name"
              required
            ></v-text-field>
            <v-text-field
              v-model="phone"
              :rules="phoneRules"
              label="Phone"
              mask="phone"
              return-masked-value
              required
            ></v-text-field>
            <div class="guest-wrapper">
              <p class="title font-weight-bold">Guest</p>
              <v-btn
                fab
                dark
                small
                color="indigo"
                @click="add"
              >
                <v-icon dark>add</v-icon>
              </v-btn>
            </div>
            <div class="guest-wrapper" v-for="(guest, index) in guests" :key="index">
              <v-text-field
                :ref='"guestName" + index'
                :value='guest.name'
                :rules="guestRules"
                :label="'Guest ' + (index + 1)"
                required
                @keyup='updateItem(index)'
                @blur='updateItem(index)'
                @paste='updateItem(index)'
                @delete='updateItem(index)'
              ></v-text-field>
              <v-btn fab dark small color="red" @dblclick="remove">
                <v-icon dark>remove</v-icon>
              </v-btn>
            </div>
            <v-btn
              color="success"
              :disabled="!valid"
              @click="send"
            >Send Data</v-btn>
          </v-form>
          <v-data-table
            :headers="headers"
            :items="table"
            class="elevation-1"
          >
            <template
              slot="items"
              slot-scope="props"
            >
              <td class="text-xs-left">{{ props.item.email }}</td>
              <td class="text-xs-left">{{ props.item.name }} {{ props.item.lastName }}</td>
              <td class="text-xs-left">{{ props.item.phone }}</td>
              <td class="text-xs-left">{{ props.item.guest }}</td>
            </template>
          </v-data-table>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "HelloWorld",
  data: () => ({
    valid: true,
    email: "",
    name: "",
    lastName: "",
    phone: "",
    nameRules: [v => !!v || "Name is required"],
    guestRules: [v => !!v || "Guest is required"],
    emailRules: [
      v => !!v || "E-mail is required",
      v => /.+@.+/.test(v) || "E-mail must be valid"
    ],
    phoneRules: [v => !!v || "Phone is required"],
    table: [],
    guests: [],
    headers: [
      { text: "Email", value: "Email" },
      { text: "Name", value: "Name" },
      { text: "Phone", value: "Phone" },
      { text: "Guests", value: "Guests" }
    ]
  }),
  methods: {
    updateItem(index) {
      this.guests[index].name =  this.$refs["guestName" + index][0].internalValue;
    },
    add: function() {
      this.guests.push({
        name: 'Guest name'
      });
    },
    send: function() {
      if (this.$refs.form.validate()) {
        this.table.push({
          email: this.email,
          name: this.name,
          lastName: this.lastName,
          phone: this.phone,
          guest: this.guests.map(x => x.name).join(', ')
        });

        this.$refs.form.reset();
        this.guests = [];
      }
    },
    remove(index) {
      this.guests.indexOf(index);
      this.guests.splice(index, 1)
    }
  }
};
</script>


<style lang="sass">
.guest-wrapper
  display: flex
  align-items: center
  margin-bottom: 20px

  p
    margin: 0 20px 0 0

hr
  margin-bottom: 20px

.elevation-1
  margin-top: 35px
</style>
