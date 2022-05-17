<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="6">
          <h2>Sorting Training System</h2>
        </v-col>
        <v-col cols="6">
          <v-dialog v-model="dialog" persistent max-width="600px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                depressed
                dark
                color="#FF8D00"
                class="float-end"
                v-bind="attrs"
                v-on="on"
              >
                Start sorting!
              </v-btn>
              <div>
                <v-btn @click="stop" depressed dark color="#424242" class="mb-2"
                  >Stop</v-btn
                >   
                <!-- <button @click="reset">Reset</button> -->
                <p>{{ formattedElapsedTime }}</p>
              </div>
            </template>                                                                                                 
            <!-- Modals -->
            <v-card>
              <v-card-title>
                <span class="text-h5">How many people?</span>
              </v-card-title>
              <v-divider></v-divider>
              <v-card-text>
                <v-container>
                  <div>
                    <p>
                      Enter a number of how many people you want to add to the
                      list.
                    </p>
                  </div>
                  <v-row>
                    <v-col cols="12" sm="6" md="12">
                      <v-text-field
                        type="number"
                        outlined
                        required
                        v-model="people_input"
                        :rules="rules"
                      >
                      </v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed @click="dialog = false"> Cancel </v-btn>
                <v-btn color="#FF8D00" depressed dark @click="add_number">
                  Start
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>
      <!-- Table -->
      <v-row>
        <v-col cols="12">
          <v-data-table
            v-model="selected"
            :headers="headers"
            :items="desserts"
            item-key="name"
            show-select
            class="elevation-1"
            :hide-default-footer="true"
            disable-pagination
          >
            <template v-slot:body="props">
              <draggable :list="props.items" tag="tbody" @change="end_drag">
                <tr v-for="(user, index) in props.items" :key="index">
                  <td>
                    <!-- <v-icon small class="page__grab-icon">
                      mdi-arrow-all
                    </v-icon> -->
                    <v-checkbox :single-select="singleSelect"></v-checkbox>
                  </td>

                  <td>{{ user.name }}</td>
                  <td>{{ user.potatos }}</td>
                  <td>
                    <v-chip class="ma-2"> Customers </v-chip>
                  </td>
                  <td>{{ user.full_name }}</td>
                  <td>
                    <p>Lithuania</p>
                  </td>
                </tr>
              </draggable>
            </template>
          </v-data-table>
        </v-col>
      </v-row>

      <!-- Modals -->
      <v-row justify="center"> </v-row>
    </v-container>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "PeopleSorting",

  data() {
    return {
      people_input: "",
      dialog: false,
      singleSelect: false,
      selected: [],
      rules: [
        (v) => parseInt(v) <= 100 || "Invalid input. Input should be 20 to 100",
        (v) => parseInt(v) >= 20 || "Invalid input. Input should be 20 to 100",
      ],
      headers: [
        {
          text: "Email",
          key: "email",
          align: "start",
          sortable: false,
          value: "name",
        },
        {
          text: "Potatoes",
          value: "potatos",
        },
        {
          key: "tags",
          text: "Tags",
          value: "tag",
        },
        {
          text: "Full name",
          value: "full_name",
        },
        {
          text: "Location",
          value: "location",
        },
      ],
      desserts: [],
      // timer
      elapsedTime: 0,
      timer: undefined,
    };
  },
  components: {
    draggable,
  },
  computed: {
    formattedElapsedTime() {
      const date = new Date(null);
      date.setSeconds(this.elapsedTime / 1000);
      const utc = date.toUTCString();
      return utc.substr(utc.indexOf(":") - 2, 8);
    },
  },

  mounted() {
    // this.showRemaining();
  },

  methods: {
    add_number() {
      if (
        parseInt(this.people_input) <= 100 &&
        parseInt(this.people_input) >= 20
      ) {
        // Random Email start
        var result = "";
        var characters = "abcdefghijklmnopqrstuvwxyz0123456789";
        var charactersLength = characters.length;
        for (let index = 0; index < parseInt(this.people_input); index++) {
          var result = "";
          for (var i = 0; i < 10; i++) {
            result += characters.charAt(
              Math.floor(Math.random() * charactersLength)
            );
          }
          // Random email end

          // Random Name
          var text = "";
          var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";

          for (var i = 0; i < 5; i++)
            text += possible.charAt(
              Math.floor(Math.random() * possible.length)
            );

          this.desserts.push({
            name: result + "@gmail.com",
            potatos: index + 1,
            tag: 6.0,
            full_name: text,
            location: 4.0,
          });
        }
      }
      this.dialog = false;
      this.start();
    },
    start() {
      this.timer = setInterval(() => {
        this.elapsedTime += 1000;
      }, 1000);
    },
    stop() {
      clearInterval(this.timer);
    },
    reset() {
      this.elapsedTime = 0;
    },
    end_drag() {
      // this.stop();
      this.desserts;
      console.log(this.desserts);
    },
  },
};
</script>
<style>
body {
  cursor: move;
}
</style>