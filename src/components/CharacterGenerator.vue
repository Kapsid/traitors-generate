<template>
  <div class="min-h-screen bg-black text-green-200 py-6 px-4">
    <div class="max-w-7xl mx-auto bg-black text-green-200 shadow-lg p-6 rounded-lg">
      <div class="flex justify-center mb-6">
        <img src="/logo.png" alt="Logo" class="h-24 w-auto">
      </div>
      <h1 class="text-2xl font-bold text-center mb-6">
        Zrádci - Generátor Hráčů
      </h1>
      <button
          @click="generatePlayers"
          class="w-full bg-green-700 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-lg mb-6 transition duration-300"
      >
        Vygenerovat Hráče
      </button>

      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6">
        <div
            v-for="player in players"
            :key="player.id"
            class="p-4 rounded-lg shadow-lg text-center bg-green-900 hover:shadow-xl transition duration-300 border border-green-700"
            :class="player.isTraitor ? 'border-red-500' : ''"
        >
          <div class="flex flex-col items-center">
            <chracter-data
                :age="player.age"
                :gender="player.gender"
                :image="player.image"
                :is-traitor="player.isTraitor"
                :name="player.name"
                :occupation="player.occupation"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import names from "@/data/names.json";
import occupations from "@/data/occupations.json";
import ChracterData from "@/components/ChracterData";

export default {
  components: {ChracterData},
  data() {
    return {
      players: [],
      genders: ["Muž", "Žena"],
    };
  },
  methods: {
    generatePlayers() {
      // Vygeneruj hráče
      const players = Array.from({ length: 24 }, (_, index) => {
        const isNickname = Math.random() < 0.2; // 20% has nickname
        const name = isNickname
            ? names.nicknames[Math.floor(Math.random() * names.nicknames.length)]
            : names.fullNames[Math.floor(Math.random() * names.fullNames.length)];
        const gender = this.genders[Math.floor(Math.random() * this.genders.length)];
        const imageSeed = `${gender}-${name}-${index}`;

        return {
          id: index + 1,
          name,
          age: Math.floor(Math.random() * 50) + 20,
          gender,
          occupation: occupations[Math.floor(Math.random() * occupations.length)],
          image: `https://api.dicebear.com/6.x/avataaars/svg?seed=${imageSeed}&gender=${
              gender === "Muž" ? "male" : "female"
          }`,
          isTraitor: false,
        };
      });

      // Choose traitors
      const traitorCount = Math.floor(Math.random() * 3) + 2;
      const traitorIndices = this.shuffleArray([...Array(24).keys()]).slice(0, traitorCount);

      traitorIndices.forEach((index) => {
        players[index].isTraitor = true;
      });

      // Seřaď hráče: nejdříve zrádci, pak věrní
      this.players = players.sort((a, b) => b.isTraitor - a.isTraitor);
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
  },
  mounted() {
    this.generatePlayers();
  },
};
</script>
