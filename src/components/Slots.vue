<template>
  <v-row>
    <v-col cols="12" sm="7" offset-sm="2">
      <v-card>
        <v-container fluid>
          <v-row>
            <v-col v-for="n in slots" :key="n.id" class="d-flex child-flex" cols="4">
              <Slot :target="n.target" :index="n.id" v-on:whacked="whacked" v-on:lose="lose" />
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import Slot from "./Slot";
    export default {
        components: {
            Slot
        },
        props: {
            slots: Array
        },
        methods: {
            whacked(index) {
                if(this.slots[index].target) {
                    this.$emit("scored", index);
                }
            },
            lose() {
                this.$emit("losed");
            }
        },
    }
</script>