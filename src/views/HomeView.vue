<template>
  <div>
    <v-container grid-list-xs mt-5>
      <v-row>
        <v-col>
          <v-card class="mx-auto" max-width="1400" outlined>
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="text-h5 mb-1">
                  <h3>Messages</h3>
                </v-list-item-title>
                <v-row>
                  <v-col md="2">
                    <v-card-text>Mobile ID</v-card-text>
                  </v-col>
                  <v-col>
                    <v-text-field v-model="mobileId" name="name"></v-text-field>
                  </v-col>
                  <v-col md="2">
                    <v-card-text>Start UTC</v-card-text>
                  </v-col>
                  <v-col>
                    <v-text-field v-model="startUtc" name="name"></v-text-field>
                  </v-col>
                </v-row>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn @click="getAllData()" outlined rounded text> Check </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

      </v-row>
      <v-row>
        <v-col>
          <v-card v-if="getFromUrl" class="mx-auto" max-width="1400" outlined>

            <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">Timestamp</th>
                    <th class="text-left">Sender ID</th>
                    <th class="text-left">Receiver ID</th>
                    <th class="text-left">Message</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in getFromUrl" :key="item.chat.timestamp">
                    <td>{{ item.chat.timestamp }}</td>
                    <td>{{ item.chat.senderId }}</td>
                    <td>{{ item.chat.receiverId }}</td>
                    <td>{{ item.chat.message }}</td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
            
          </v-card>
        </v-col>
      </v-row>
      <v-overlay :value="overlay">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    overlay: false,
    getFromUrl: null,
    chatMessages: null,
    mobileId: "01434579SKY2D5C",
    startUtc: "2022-07-15",
  }),
  methods: {
    getAllData() {
      this.overlay = true
      const getUrl = `http://localhost:3000/messages?mobileid=${this.mobileId}&startUtc=${this.startUtc} 06:00:00`
      axios.get(getUrl)
        .then((response) => {
          console.log(response);
          this.getFromUrl = response.data
          this.chatMessages = this.getFromUrl[0].chat
          console.log(this.chatMessages);
          this.overlay = false
        })
        .catch((error) => {
          console.log(error);
        })
    }
  },
}
</script>
