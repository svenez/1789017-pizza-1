<template>
  <div class="content__constructor">
    <AppDrop @drop="addIngredient($event)">
      <div
        :class="`pizza pizza--foundation--${doughNormalized}-${pizza.sauce}`"
      >
        <div class="pizza__wrapper">
          <div
            v-for="(classes, i) in ingredientsClasses"
            :key="i"
            :class="classes"
          ></div>
        </div>
      </div>
    </AppDrop>
  </div>
</template>

<script>
import { AppDrop } from "@/common/components/behavior";
import { mapMutations, mapState } from "vuex";
import { UPDATE_PIZZA_INGREDIENT } from "@/store/mutations-types";

export default {
  name: "BuilderPizzaView",
  components: { AppDrop },
  computed: {
    ...mapState("Builder", {
      pizza: "pizza",
    }),

    /**
     * Converting dough codes
     * */
    doughNormalized() {
      return {
        light: "small",
        large: "big",
      }[this.pizza.dough];
    },

    /**
     * Preparing pizza ingredients display classes
     * @returns {*[]}
     */
    ingredientsClasses() {
      const res = [];

      const countClasses = ["", "second", "third"];
      this.pizza.ingredients.forEach((ingredient) => {
        for (let i = 0; i < ingredient.count; i++) {
          const countClass = countClasses[i],
            classes = ["pizza__filling", `pizza__filling--${ingredient.code}`];
          if (countClass) classes.push(`pizza__filling--${countClass}`);
          res.push(classes);
        }
      });

      return res;
    },
  },
  methods: {
    ...mapMutations("Builder", {
      setPizzaIngredient: UPDATE_PIZZA_INGREDIENT,
    }),
    addIngredient({ code }) {
      this.setPizzaIngredient({ code, count: 1 });
    },
  },
};
</script>

<style scoped></style>
