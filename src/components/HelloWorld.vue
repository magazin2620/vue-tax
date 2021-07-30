<template>
  <v-container>
    <v-row>
      <v-col class="d-flex justify-center text-body-2" cols="12">
        <v-sheet
          class="d-flex justify-center align-center"
          color="grey lighten-4"
          elevation="7"
          min-height="420"
          min-width="420"
          max-width="550"
        >
          <v-sheet
            class="pa-5 ma-4"
            elevation="2"
            min-height="400"
            min-width="400"
            max-width="530"
          >
            <div class="text-subtitle-1 font-weight-medium">
              Добавление организации
            </div>
            <v-row class="d-flex align-center">
              <v-col cols="3">Организация</v-col>
              <v-col cols="9">
                <v-autocomplete
                  :items="items"
                  clearable
                  :search-input.sync="search"
                  item-text="value"
                ></v-autocomplete>
              </v-col>
            </v-row>
            <div class="text-subtitle-1 font-weight-medium mb-5">Реквизиты</div>
            <v-row class="d-flex align-top">
              <v-col cols="3">Название</v-col>
              <v-col cols="9" v-for="(item, i) in items" :key="i">{{
                item.value
              }}</v-col>
            </v-row>
            <v-row class="d-flex align-center">
              <v-col cols="3">ИНН</v-col>
              <v-col cols="9" v-for="(item, i) in items" :key="i">{{
                item.inn
              }}</v-col>
            </v-row>
            <v-row class="d-flex align-center">
              <v-col cols="3">КПП</v-col>
              <v-col cols="9" v-for="(item, i) in items" :key="i">{{
                item.kpp
              }}</v-col>
            </v-row>
            <v-row class="d-flex align-center">
              <v-col cols="3">ОГРН</v-col>
              <v-col cols="9" v-for="(item, i) in items" :key="i">{{
                item.ogrn
              }}</v-col>
            </v-row>
            <v-row class="d-flex align-top">
              <v-col cols="3">Юр. адрес</v-col>
              <v-col cols="9" v-for="(item, i) in items" :key="i">{{
                item.address
              }}</v-col>
            </v-row>
            <div>{{ this.items }}</div>
          </v-sheet>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      items: [],
      search: null,
    };
  },
  watch: {
    search() {
      axios
        .get(
          "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/party?Content-Type=application/json&token=5de7b238a1fdc912edd8b15084d44da035ff59ff&query=" +
            this.search
        )
        .then((response) => {
          this.items = response.data.suggestions.map((item) => ({
            value: item.value,
            inn: item.data.inn,
            kpp: item.data.kpp,
            ogrn: item.data.ogrn,
            address: item.data.address.unrestricted_value,
          }));
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.v-autocomplete label {
  font-size: 0.8em;
  margin-left: 5px;
}
</style>