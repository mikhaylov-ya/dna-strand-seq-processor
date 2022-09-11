<script setup>
  import { ref, reactive, onRenderTriggered } from "vue";

  const state = reactive({
    sequence: '',
    errorMessage: 'Enter valid DNA sequences using only A, T, G, C',
    isValid: true,
  });

  const input = ref(null);

  onRenderTriggered(() => {
    state.isValid = !input.value.validity.patternMismatch;
  });

  const map = {
    A: 'T',
    T: 'A',
    C: 'G',
    G: 'C',
  };

  const SeqLength = 13;

</script>

<template>
  <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="w-full max-w-md space-y-8">
        <div class="-space-y-px rounded-md shadow-sm">
          <div class="container">
            <p class="my-5 text-4xl font-bold tracking-tight text-gray-900">Enter a DNA sequence</p>
            <p class="text-red-500 py-1" v-if="!state.isValid">{{ state.errorMessage }}</p>
            <p class="text-gray-500 py-1" v-else>Sequence is fine</p>
            <input
              ref="input"
              autofocus
              size="100"
              v-model="state.sequence"
              pattern="[ATGCatgc]{0,}"
              title="Wrong nucleo designation"
              maxlength="100"
              id="dna"
              name="dna"
              type="text"
              placeholder="DNA Sequence"
              class="text-xs block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 invalid:border-red-500 invalid:border-4" />
          <div class="w-sm h-lg my-4 p-5 overflow-y-auto rounded-t-md border-blue-500 border-4">
            <span v-for="(char, i) in state.sequence" :key="i" class="inline-block">
              {{ map[char.toUpperCase()] }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<!-- От вас требуется собрать интерфейс-форму. В форме должен быть инпут для
задания последовательности нуклеотидов во фрагменте одной из двух цепочек
ДНК.
Форма должна уметь:

1. Реактивно (сразу реагируя на изменения) дорисовывать нуклеотиды из
комплементарного фрагмента цепочки (например, можно под инпутом
дорисовывать оба фрагмента цепочки). Комплементарные пары предлагается
вспомнить самостоятельно или уточнить загуглив.

2. Реактивно отображать температуру плавления заданной последовательности.
“Плавление“ в данном случае означает разрыв всех водородных связей между
комплементарными нуклеотидами. Можно исходить из предположения, что
зависимость температуры плавления от нуклеотидного состава описывается
формулой:
T= 64.9 +41(N(G)+N(C) - 16.4) / (N(A)+N(T)+N(G)+N(C))
для последовательностей длины не меньше 13.
Для последовательностей меньшей длины, используется формула
T= 2 * (N(A) + N(T)) + (N(G) + N(C)) * 4
где N(letter) - количество нуклеотида letter во введенной
последовательности (только в одном из двух фрагментов).
3. Предположим, что данная формула работает хорошо для последовательностей
не длиннее 100 нуклеотидов, поэтому можно ограничить длину вводимой
последовательности сотней символов.
Кроме того, будет разумно сделать так, чтобы форма не пускала любые
символы, кроме “A”, “T”, “G”, “C” или ругалась на их присутствие.

Желательно реализовать на Vue, заодно разобравшись с основами написания
шаблонов и компонентов. Так же, рекомендуется организовать работу компонента с формой так,
чтобы в нем использовались стандартные инструменты для динамической генерации контента (встроенная директива v-for, например, чтобы комплементарные пары нуклеотидов отображались с помощью этой директивы)
В ответ можете прислать ссылку на репозиторий с проектом,
чтобы его можно было склонировать и запустить локально.
 -->
