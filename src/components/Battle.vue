<template>
  <v-container>
    <v-card
      flat
      class="d-flex justify-space-around mb-6"
    >
      <div class="d-flex flex-column align-center">
        <h4>Jogador</h4>
        <v-img
          v-if="resultado === 'lose'"
          class="my-0"
          src="@/assets/rip.png"
          contain
          height="250"
          width="150"
        >
        </v-img>
        <v-img
          v-else
          class="my-0"
          src="@/assets/knight.png"
          contain
          height="250"
          width="150"
        >
        </v-img>
        <v-progress-linear
          :value="vidaJogador"
          style="width: 300px;"
          :color="progressColorPlayer"
          height="22"
        >
          <template v-slot:default="{ value }">
            <strong>{{ Math.ceil(value) }}%</strong>
          </template>
        </v-progress-linear>
      </div>
      <div class="d-flex flex-column align-center">
        <h4>Monstro</h4>
        <v-img
          v-if="resultado === 'win'"
          class="my-0"
          src="@/assets/rip.png"
          contain
          height="250"
          width="150"
        >
        </v-img>
        <v-img
          v-else
          class="my-0"
          src="@/assets/monster.png"
          contain
          height="250"
          width="150"
        >
        </v-img>
        <v-progress-linear
          :value="vidaMonstro"
          style="width: 300px;"
          :color="progressColorMonster"
          height="22"
        >
          <template v-slot:default="{ value }">
            <strong>{{ Math.ceil(value) }}%</strong>
          </template>
        </v-progress-linear>
      </div>
    </v-card>

    <v-card flat>
      <div
        v-if="start"
        class="d-flex flex-column align-center"
      >
        <v-card width="600">
          <v-card-actions class="d-flex justify-center">
            <v-btn
              color="primary"
              class="my-4"
              @click="novoJogo"
            >INICIAR NOVO JOGO</v-btn>
          </v-card-actions>
          <v-card-text>
            <h4 class="mx-0 d-flex justify-center">
              Este jogo requer sorte e estratégia para vencer o monstro.
            </h4>
            <p class="my-1"> Regras:</p>
            <p class="my-1">
              - Ataque: Seu jogador ataca o monstro mas, em contra-partida o monstro lhe ataca de volta. Neste ataque o monstro é mais forte que você.
            </p>
            <p class="my-1">
              - Ataque Especial: Neste ataque, você é mais forte que o montro mas, seus ataques especiais são limitados. Use-os com sabedoria.
            </p>
            <p class="my-1">
              - Curar: Seu jogador recebe cura. Em contra-partida recebe um ataque do monstro. Sua cura também é limitada.
            </p>
            <p class="my-0">
              - Desistir: Reinicia o jogo.
            </p>
          </v-card-text>
        </v-card>
      </div>

      <div
        v-else
        class="d-flex justify-center my-2"
      >
        <v-btn
          class="mx-2"
          color="primary"
          @click="ataque"
        >ATAQUE</v-btn>
        <v-btn
          color="#536DFE"
          :disabled="disableSpecial"
          class="mx-2"
          @click="ataqueEspecial"
        >ATAQUE ESPECIAL ({{contSpecial}}x)</v-btn>
        <v-btn
          :disabled="disableCura"
          color="success"
          class="mx-2"
          @click="curar"
        >CURAR ({{contCura}}x)</v-btn>
        <v-btn
          color="#607D8B"
          class="mx-2"
          @click="desistir"
        >DESISTIR</v-btn>
      </div>
    </v-card>

    <v-card
      flat
      class="d-flex justify-center my-2"
    >
      <ul>
        <li
          style="display: flex; flex-direction: column"
          v-for="(log, i) in logs"
          :key="i"
        >{{ log }}
          <v-divider></v-divider>
        </li>
      </ul>
    </v-card>

  </v-container>
</template>

<script>
export default {
  name: 'Battle',

  data: () => ({
    start: true,

    vidaJogador: 100,
    vidaMonstro: 100,

    progressColorPlayer: 'success',
    progressColorMonster: 'success',

    resultado: '',

    danoJogador: 0,
    danoMonstro: 0,
    curaJogador: 0,

    logs: [],

    contSpecial: 5,
    contCura: 3,
    disableSpecial: false,
    disableCura: false

  }),

  methods: {

    novoJogo () {
      this.start = false
      this.result = false
      this.progressColorPlayer = 'success'
      this.progressColorMonster = 'success'
      this.vidaJogador = 100
      this.vidaMonstro = 100
      this.logs = []
      this.disableSpecial = false
      this.disableCura = false
      this.contSpecial = 5
      this.contCura = 3
      this.resultado = ''
    },

    verifica () {
      this.logs = []
      if (this.contSpecial === 0) {
        this.disableSpecial = true
      }
      if (this.contCura === 0) {
        this.disableCura = true
      }
      if (this.vidaJogador >= 40) {
        this.progressColorPlayer = 'success'
      }
      if (this.vidaJogador > 20 && this.vidaJogador < 40) {
        this.progressColorPlayer = 'amber'
      }
      if (this.vidaJogador < 40 && this.vidaJogador > 20) {
        this.progressColorPlayer = 'amber'
      }
      if (this.vidaMonstro < 40 && this.vidaMonstro > 20) {
        this.progressColorMonster = 'amber'
      }
      if (this.vidaJogador < 20) {
        this.progressColorPlayer = 'error'
      }
      if (this.vidaMonstro < 20) {
        this.progressColorMonster = 'error'
      }
      if (this.vidaMonstro <= 0) {
        this.vidaMonstro = 0
        this.result = true
        this.resultado = 'win'
        this.start = true
      }
      if (this.vidaMonstro <= 0 && this.vidaJogador <= 0) {
        this.vidaMonstro = 0
        this.vidaJogador = 1
        this.result = true
        this.resultado = 'win'
        this.start = true
      }
      if (this.vidaJogador <= 0) {
        this.vidaJogador = 0
        this.result = true
        this.resultado = 'lose'
        this.start = true
      }
    },

    ataque () {
      this.danoJogador = ((Math.floor(Math.random() * 5) + 1) * 3)
      this.danoMonstro = ((Math.floor(Math.random() * 3) + 1) * 3)
      this.vidaJogador -= this.danoJogador
      this.vidaMonstro -= this.danoMonstro
      this.verifica()
      const p = `Jogador recebeu ${this.danoJogador} pontos de dano`
      const m = `Monstro recebeu ${this.danoMonstro} pontos de dano`
      this.logs.unshift(p)
      this.logs.unshift(m)
    },

    ataqueEspecial () {
      this.contSpecial = this.contSpecial - 1
      this.danoJogador = ((Math.floor(Math.random() * 3) + 1) * 3)
      this.danoMonstro = ((Math.floor(Math.random() * 5) + 1) * 3)
      this.vidaJogador -= this.danoJogador
      this.vidaMonstro -= this.danoMonstro
      this.verifica()
      const p = `Jogador recebeu ${this.danoJogador} pontos de dano`
      const m = `Monstro recebeu ${this.danoMonstro} pontos de dano`
      this.logs.unshift(p)
      this.logs.unshift(m)
    },

    curar () {
      this.contCura = this.contCura - 1
      this.curaJogador = ((Math.floor(Math.random() * 5) + 1) * 3)
      this.danoMonstro = ((Math.floor(Math.random() * 3) + 1) * 3)
      this.vidaJogador += this.curaJogador
      this.vidaJogador -= this.danoMonstro
      this.verifica()
      const p = `Jogador recebeu ${this.curaJogador} pontos de vida`
      const m = `Jogador recebeu ${this.danoMonstro} pontos de dano`
      this.logs.unshift(p)
      this.logs.unshift(m)
    },

    desistir () {
      this.start = true
      this.result = false
      this.progressColorPlayer = 'success'
      this.progressColorMonster = 'success'
      this.vidaJogador = 100
      this.vidaMonstro = 100
      this.logs = []
      this.disableSpecial = false
      this.contSpecial = 5
      this.contCura = 3
    }

  }
}
</script>
