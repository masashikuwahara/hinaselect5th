<template>
  <div v-if="!finished">
    <Battle :player1="player1" :player2="player2" @winnerSelected="selectWinner" @draw="drawMatch" />
  </div>
  <div v-if="finished">
    <Ranking :members="sortedMembers" @resetGame="resetGame" />
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import Battle from "@/components/Battle.vue";
import Ranking from "@/components/Ranking.vue";

export default {
  components: { Battle, Ranking },
  setup() {
    const membersInitialState = [
      { id: 1, name: "大野愛実", image: "https://cdn.hinatazaka46.com/images/14/de6/51f1b2474317dbf1f8883964d91f6-03.jpg", score: 0 },
      { id: 2, name: "鶴崎仁香", image: "https://cdn.hinatazaka46.com/images/14/2d0/201d8f3005ee07574c8ce7db5cb16-03.jpg", score: 0 },
      { id: 3, name: "坂井新奈", image: "https://cdn.hinatazaka46.com/images/14/c9d/87011522b663e9b274bacf7644c16-03.jpg", score: 0 },
      { id: 4, name: "佐藤優羽", image: "https://cdn.hinatazaka46.com/images/14/d51/52c019274111cb3b916a0f8aac3b8-03.jpg", score: 0 },
      { id: 5, name: "下田衣珠季", image: "https://cdn.hinatazaka46.com/images/14/e2d/cd9b08205df9b479d76e1e6be9a70-03.jpg", score: 0 },
      { id: 6, name: "片山紗希", image: "https://cdn.hinatazaka46.com/images/14/37a/d13677487443abce1aac730b8e2c4-03.jpg", score: 0 },
      { id: 7, name: "大田美月", image: "https://cdn.hinatazaka46.com/images/14/bfb/e0ddea499c10c4aaf5d3c4a4524f1-03.jpg", score: 0 },
      { id: 8, name: "高井俐香", image: "https://cdn.hinatazaka46.com/images/14/399/71117b1d7bafbd32e4f8b7421517b-03.jpg", score: 0 },
      { id: 9, name: "松尾桜", image: "https://cdn.hinatazaka46.com/images/14/389/6768010684b4668881ef3651c1732-05.jpg", score: 0 },
      { id: 10, name: "蔵盛妃那乃", image: "https://cdn.hinatazaka46.com/images/14/55e/587c555a3dcfc93e8eb67473e0a6e-05.jpg", score: 0 }
    ];

    const members = ref([...membersInitialState]);
    const matches = ref(20);
    const currentRound = ref(0);
    const finished = ref(false);
    const player1 = ref(null);
    const player2 = ref(null);

    const sortedMembers = computed(() => [...members.value].sort((a, b) => b.score - a.score));

    const startMatch = () => {
      if (currentRound.value >= matches.value) {
        finished.value = true;
        return;
      }

      let sorted = sortedMembers.value;
      if (sorted.length < 2) {
        finished.value = true;
        return;
      }

      let index1 = Math.floor(Math.random() * sorted.length / 2);
      let index2 = index1 + Math.floor(sorted.length / 2);

      player1.value = sorted[index1] || sorted[0];
      player2.value = sorted[index2] || sorted[1];

      currentRound.value++;
    };

    const selectWinner = (winner) => {
      winner.score += 1;
      startMatch();
    };

    const drawMatch = () => {
      startMatch();
    };

    const resetGame = () => {
      members.value = [...membersInitialState];
      matches.value = 20;
      currentRound.value = 0;
      finished.value = false;
      player1.value = null;
      player2.value = null;
      startMatch();
    };

    onMounted(() => {
      startMatch();
    });

    return { player1, player2, selectWinner, drawMatch, finished, sortedMembers, resetGame };
  }
};
</script>
