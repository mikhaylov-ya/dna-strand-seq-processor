<script setup>
import { ref, reactive, computed, watch } from "vue";

  const state = reactive({
    sequence: '',
    meltingTemp: 0,
    isValid: true,
  });


  const input = ref(null);

  const errorMessage = 'Enter valid DNA sequences using only A, T, G, C';
  const pattern = /[ATGCatgc]*/;
  const map = {
    A: 'T',
    T: 'A',
    C: 'G',
    G: 'C',
  };

  const handleInput = (e) => {
    state.isValid = !e.target.validity.patternMismatch;
    state.sequence = normalize(e.target.value);
  };

  const normalize = (seq) => {
    const [str] = seq.match(pattern);
    return str.toUpperCase();
  };

  const output = computed(() => state.sequence.split('')
    .map((el) => map[el.toUpperCase()])
    .filter(Boolean));

  watch(state, (newState) => {
    const { sequence } = newState;
    const { A = 0, T = 0, G = 0, C = 0 } = sequence.split('').reduce((acc, el) => {
      acc[el] = (acc[el] ?? 0 ) + 1;
      return acc;
    }, {});

    if (sequence.length >= 13) {
      state.meltingTemp = (64.9 + 41 * (G + C - 16.4) / (A + T + G + C));
    } else {
      state.meltingTemp = ((A + T) * 2 + (G + C) * 4);
    }
  });

</script>

<template>
  <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="w-full max-w-md space-y-8">
      <div class="-space-y-px rounded-md shadow-sm">
        <div class="container">
          <p class="my-5 text-4xl font-bold tracking-tight text-gray-900">Enter a DNA strand nucleotide sequence</p>
          <p class="text-red-500 py-1" v-if="!state.isValid">{{ errorMessage }}</p>
          <p class="text-gray-500 py-1" v-else>Sequence is fine</p>
          <input
            ref="input"
            autofocus
            size="100"
            :value="state.sequence"
            @input="event => handleInput(event)"
            :pattern="pattern.source"
            title="Nucleotides"
            maxlength="100"
            id="dna"
            name="dna"
            type="text"
            placeholder="Nucleotide sequence"
            class="text-xs block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 invalid:border-red-500 invalid:border-4" />
          <p class="italic mt-4">Complementary strand:</p>
          <div class="w-sm h-lg my-4 p-5 overflow-y-auto rounded-t-md border-emerald-600 border-2">
          <span v-for="(char, i) in output" :key="i" class="inline-block">
            {{ char }}
          </span>
          </div>
          <div class="w-sm h-lg my-4 p-5 overflow-y-auto rounded-t-md border-gray-500 border-3">
            <p>Melting temperature: {{ state.meltingTemp.toFixed(2) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
