<script setup lang="ts">
import { ref, watch } from "vue";
import QrCode from "./components/qr-code.vue";
import Title from "./components/title.vue";
import Content from "./components/content.vue";
import Spoiler from "./components/spoiler.vue";
import SmallCase from "./components/small-case.vue";
type Case = 0 | 1;
const cases: Case[][] = [
  [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
  [0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0],
  [0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0],
  [0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0],
  [0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0],
  [0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0],
  [0, 0, 0, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0],
  [0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0],
  [0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0],
  [0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0],
  [0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 1, 0],
  [0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0],
  [0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0],
  [0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 0, 0, 0],
  [0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 0],
  [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
];
const isLocator = (row: number, col: number): boolean => {
  return (row >= 1 && row <= 7 && ((col >= 1 && col <= 7) || (col >= cases[row].length - 8 && col <= cases[row].length - 2))) || (row >= cases.length - 8 && row <= cases.length - 2 && col >= 1 && col <= 7);
};
const isQuietZone = (row: number, col: number): boolean => {
  return row === 0 || col === 0 || row === cases.length - 1 || col === cases[row].length - 1;
};
const isTiming = (row: number, col: number): boolean => {
  return (row === 7 && col >= 9 && col <= cases[row].length - 10) || (col === 7 && row >= 9 && row <= cases.length - 10) || (row === cases.length - 9 && col === 9);
};
const isSeparator = (row: number, col: number): boolean => {
  return (
    (row === 8 && ((col >= 1 && col <= 8) || (col >= cases[row].length - 9 && col <= cases[row].length - 2))) ||
    (col === 8 && ((row >= 1 && row <= 8) || (row >= cases.length - 9 && row <= cases.length - 2))) ||
    (col === cases[row].length - 9 && row >= 1 && row <= 8) ||
    (row === cases.length - 9 && col >= 1 && col <= 8)
  );
};
const isErrorCorrectionLevel = (row: number, col: number): boolean => {
  return (row === 9 && (col === 1 || col === 2)) || (col === 9 && (row === cases.length - 2 || row === cases.length - 3));
};
const isMaskPattern = (row: number, col: number): boolean => {
  return (row === 9 && col >= 3 && col <= 5) || (col === 9 && row >= cases.length - 6 && row <= cases.length - 4);
};
const isFormatErrorCorrection = (row: number, col: number): boolean => {
  return (
    (row === 9 && col === 6) ||
    (col === 9 && ((row >= 1 && row <= 9) || (row >= cases.length - 8 && row <= cases.length - 7))) ||
    (row === 9 && ((col >= 8 && col <= 9) || (col >= cases[row].length - 9 && col <= cases[row].length - 2)))
  );
};
const isData = (row: number, col: number): boolean =>
  !isLocator(row, col) && !isQuietZone(row, col) && !isTiming(row, col) && !isSeparator(row, col) && !isErrorCorrectionLevel(row, col) && !isMaskPattern(row, col) && !isFormatErrorCorrection(row, col);

const identifyAll = (row: number, col: number): string => {
  if (isLocator(row, col)) return locator;
  else if (isQuietZone(row, col)) return quietZone;
  else if (isSeparator(row, col)) return separator;
  else if (isTiming(row, col)) return timing;
  else if (isErrorCorrectionLevel(row, col)) return errorCorrectionLevel;
  else if (isMaskPattern(row, col)) return maskPattern;
  else if (isFormatErrorCorrection(row, col)) return formatErrorCorrection;
  else if (isData(row, col)) return data;
  return "";
};

const locator = "bg-red-500 opacity-70";
const quietZone = "bg-orange-600 opacity-70";
const separator = "bg-orange-500 opacity-70";
const errorCorrectionLevel = "bg-yellow-400 opacity-80";
const maskPattern = "bg-blue-500 opacity-70";
const formatErrorCorrection = "bg-purple-500 opacity-70";
const timing = "bg-pink-500 opacity-70";
const data = "bg-green-500 opacity-70";
const identifyLocator = (row: number, col: number): string => {
  if (isLocator(row, col)) return locator;
  return "";
};
const identifyErrorCorrectionLevel = (row: number, col: number): string => {
  if (isErrorCorrectionLevel(row, col)) return errorCorrectionLevel;
  return "";
};
const identifyMaskPattern = (row: number, col: number): string => {
  if (isMaskPattern(row, col)) return maskPattern;
  return "";
};
const identifyFormatErrorCorrection = (row: number, col: number): string => {
  if (isFormatErrorCorrection(row, col)) return formatErrorCorrection;
  return "";
};
const identifyTiming = (row: number, col: number): string => {
  if (isTiming(row, col)) return timing;
  return "";
};
const identifyQuietZone = (row: number, col: number): string => {
  if (isQuietZone(row, col)) return quietZone;
  else if (isSeparator(row, col)) return separator;
  return "";
};
const identifyData = (row: number, col: number): string => {
  if (isData(row, col)) return data;
  return "";
};

const hiddenCases = (value: Case): string => {
  if (value) return "bg-gray-300";
  return "bg-gray-200";
};
const xor = (maskValue: Case, qrCodeValue: Case): string => {
  return maskValue !== qrCodeValue ? "bg-black" : "bg-white";
};

const mask = (row: number, col: number, value: Case): string => {
  // different than the formula because we added the quiet zone
  if (isData(row, col)) return row % 2 === 0 ? "bg-white" : "bg-black";
  return hiddenCases(value);
};
const xored = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    return xor((row % 2) as Case, value);
  }
  return hiddenCases(value);
};

// 0100
const encoding = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    if (row === cases.length - 2 && col === cases[row].length - 2) {
      return `${clazz} border-b-4 border-r-4 border-red-500`;
    }
    if (row === cases.length - 2 && col === cases[row].length - 3) {
      return `${clazz} border-b-4 border-l-4 border-red-500`;
    }
    if (row === cases.length - 3 && col === cases[row].length - 2) {
      return `${clazz} border-t-4 border-r-4 border-red-500`;
    }
    if (row === cases.length - 3 && col === cases[row].length - 3) {
      return `${clazz} border-t-4 border-l-4 border-red-500`;
    }
    return clazz;
  }
  return hiddenCases(value);
};

const verticalSquare = (bottom: number, right: number, row: number, col: number): string => {
  if (row === cases.length - bottom && col === cases[row].length - right) {
    return `border-b-4 border-r-4 border-red-500`;
  }
  if (row === cases.length - bottom && col === cases[row].length - right - 1) {
    return `border-b-4 border-l-4 border-red-500`;
  }
  if ((row === cases.length - bottom - 1 || row === cases.length - bottom - 2) && col === cases[row].length - right) {
    return `border-r-4 border-red-500`;
  }
  if ((row === cases.length - bottom - 1 || row === cases.length - bottom - 2) && col === cases[row].length - right - 1) {
    return `border-l-4 border-red-500`;
  }
  if (row === cases.length - bottom - 3 && col === cases[row].length - right) {
    return `border-t-4 border-r-4 border-red-500`;
  }
  if (row === cases.length - bottom - 3 && col === cases[row].length - right - 1) {
    return `border-t-4 border-l-4 border-red-500`;
  }
  return "";
};

// 00001001 => 9
const length = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(4, 2, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 01100100 => 100 (d)
const firstCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(8, 2, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

const horizontalSquare = (bottom: number, right: number, row: number, col: number): string => {
  if (row === cases.length - bottom && col === cases[row].length - right) {
    return `border-b-4 border-r-4 border-red-500`;
  }
  if (row === cases.length - bottom - 1 && col === cases[row].length - right) {
    return `border-t-4 border-r-4 border-red-500`;
  }
  if (row === cases.length - bottom && (col === cases[row].length - right - 1 || col === cases[row].length - right - 2)) {
    return `border-b-4 border-red-500`;
  }
  if (row === cases.length - bottom - 1 && (col === cases[row].length - right - 1 || col === cases[row].length - right - 2)) {
    return `border-t-4 border-red-500`;
  }
  if (row === cases.length - bottom && col === cases[row].length - right - 3) {
    return `border-b-4 border-l-4 border-red-500`;
  }
  if (row === cases.length - bottom - 1 && col === cases[row].length - right - 3) {
    return `border-t-4 border-l-4 border-red-500`;
  }
  return "";
};

// 01101100 => 108 (l)
const secondCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = horizontalSquare(12, 2, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};
// 01110100 => 116 (t)
const thirdCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(8, 4, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 00100000 => 32 ( )
const fourthCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(4, 4, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 01110010 => 114 (r)
const fifthCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = horizontalSquare(2, 4, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 01101111 => 111 (o)
const sixthCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(4, 6, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 01100011 => 99 (c)
const seventhCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(8, 6, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};

// 01101011 => 107 (k)
const eighthCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = horizontalSquare(12, 6, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};
// 01110011 => 115 (s)
const ninethCharacter = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    const border = verticalSquare(8, 8, row, col);
    return `${clazz} ${border}`;
  }
  return hiddenCases(value);
};
// 0000
const endWord = (row: number, col: number, value: Case): string => {
  if (isData(row, col)) {
    const clazz = xor((row % 2) as Case, value);
    if (row === cases.length - 6 && col === cases[row].length - 8) {
      return `${clazz} border-b-4 border-r-4 border-red-500 lol`;
    }
    if (row === cases.length - 6 && col === cases[row].length - 9) {
      return `${clazz} border-b-4 border-l-4 border-red-500 lol2`;
    }
    if (row === cases.length - 7 && col === cases[row].length - 8) {
      return `${clazz} border-t-4 border-r-4 border-red-500 lol3`;
    }
    if (row === cases.length - 7 && col === cases[row].length - 9) {
      return `${clazz} border-t-4 border-l-4 border-red-500 lol4`;
    }
    return clazz;
  }
  return hiddenCases(value);
};

// https://en.wikipedia.org/wiki/QR_code
// https://en.m.wikiversity.org/wiki/Reed%E2%80%93Solomon_codes_for_coders
//npx qrcode "dlt rocks" -e L -m 1
// String.fromCharCode(0b01101100)

const steps = [
  undefined,
  identifyLocator,
  identifyQuietZone,
  identifyTiming,
  identifyErrorCorrectionLevel,
  identifyMaskPattern,
  mask,
  identifyFormatErrorCorrection,
  identifyData,
  identifyAll,
  xored,
  encoding,
  length,
  firstCharacter,
  secondCharacter,
  thirdCharacter,
  fourthCharacter,
  fifthCharacter,
  sixthCharacter,
  seventhCharacter,
  eighthCharacter,
  ninethCharacter,
  endWord,
];

const currentStepFromUrl = (Number(window.location.pathname.replace("/", "")) || 1) - 1;
const currentState = ref(Math.min(Math.max(currentStepFromUrl, 0), cases.length - 1));

watch(currentState, () => {
  const url = new URL(window.location as any);
  url.pathname = "/" + (currentState.value + 1);
  window.history.pushState({}, "", url);
});

const getStep = (expectedState: number) => {
  return currentState.value === expectedState ? steps[expectedState] : undefined;
};

const hide = (expectedState: number) => !(Math.abs(currentState.value - expectedState) > 2);

const getStepContainerClass = (expectedState: number) => {
  if (currentState.value !== expectedState) return "invisible h-0 empty";
  return "";
};
</script>

<template>
  <div class="transition-colors flex justify-center items-center flex-col">
    <div>
      <button
        v-bind:disabled="currentState <= 0"
        v-on:click="currentState--"
        class="flex-shrink-1 text-base font-semibold py-2 px-4 rounded-lg shadow-md focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 focus:ring-offset-purple-200 my-2 ml-1 mr-1"
        v-bind:class="currentState <= 0 ? 'bg-gray-200 text-gray-600 cursor-not-allowed' : 'bg-purple-600 text-white hover:bg-purple-700'"
      >
        Previous
      </button>
      <span class="border border-gray-900 py-2 px-4 rounded-lg mx-4">{{ currentState + 1 }}</span>
      <button
        v-on:click="currentState++"
        class="flex-shrink-1 text-base font-semibold py-2 px-4 rounded-lg shadow-md focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 focus:ring-offset-purple-200 my-2 ml-1 mr-1"
        v-bind:class="currentState >= steps.length - 1 ? 'bg-gray-200 text-gray-600 cursor-not-allowed' : 'bg-purple-600 text-white hover:bg-purple-700'"
      >
        Next
      </button>
    </div>
    <div class="qr-code-container">
      <div v-if="hide(0)" class="animate" v-bind:class="getStepContainerClass(0)">
        <div class="text-center"><QrCode v-bind:cases="cases" /></div>
        <div>
          <Title>Let's decode a QR Code</Title>
          <Content><a href="https://github.com/Nebulis/tech-stuff/issues/3" target="_blank" ref="noreferrer noopener">Notes</a></Content>
        </div>
      </div>
      <div v-if="hide(1)" class="animate" v-bind:class="getStepContainerClass(1)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(1)" /></div>
        <div>
          <Title>Finder Pattern</Title>
          <Content>Fixed</Content>
          <Content>Detect position of QrCode</Content>
        </div>
      </div>
      <div v-if="hide(2)" class="animate" v-bind:class="getStepContainerClass(2)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(2)" /></div>
        <div>
          <Title>Quiet Zone & Separator</Title>
          <Content>Fixed</Content>
          <Content>Separator: Whitespace beside the finder pattern</Content>
          <Content>Quiet Zone: Separate the QR code from anything else</Content>
        </div>
      </div>
      <div v-if="hide(3)" class="animate" v-bind:class="getStepContainerClass(3)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(3)" /></div>
        <div>
          <Title>Timing Pattern & Dark Module</Title>
          <Content>Fixed</Content>
          <Content>Timing patterns: Dotted lines that connect the finder patterns (6th column and 6th row)</Content>
          <Content>Dark module: Black module beside the bottom left finder pattern. ([(4 * V) + 9], 8)</Content>
        </div>
      </div>
      <div v-if="hide(4)" class="animate" v-bind:class="getStepContainerClass(4)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(4)" /></div>
        <div>
          <Title>Error Correction Level</Title>
          <Content>Allow the QR Code to be readable if alterated</Content>
          <Content>The lower the error correction level, the less dense</Content>
          <Content
            >4 levels:

            <div class="grid grid-cols-3 gap-x-2">
              <div class="justify-self-end">
                <SmallCase v-bind:value="1" />
                <SmallCase v-bind:value="1" />
              </div>
              <div class="justify-self-start col-span-2">L (7% of data bytes can be restored.)</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="1" />
                <SmallCase v-bind:value="0" />
              </div>
              <div class="justify-self-start col-span-2">M (15% of data bytes can be restored.)</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="1" />
              </div>
              <div class="justify-self-start col-span-2">Q (25% of data bytes can be restored.)</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
              </div>
              <div class="justify-self-start col-span-2">H (30% of data bytes can be restored.)</div>
            </div>
          </Content>
        </div>
      </div>
      <div v-if="hide(5)" class="animate" v-bind:class="getStepContainerClass(5)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(5)" /></div>
        <div>
          <Title>Mask Pattern</Title>
          <Content>Make the QR code easier for a QR scanner to read.</Content>
          <Content><a target="_blank" ref="noreferrer noopener" href="https://www.thonky.com/qr-code-tutorial/data-masking">Try all patterns and select the best</a></Content>
          <Content><a target="_blank" ref="noreferrer noopener" href="https://en.wikipedia.org/wiki/QR_code#/media/File:QR_Format_Information.svg">8 patterns</a></Content>
        </div>
      </div>
      <div v-if="hide(6)" class="animate" v-bind:class="getStepContainerClass(6)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(6)" /></div>
        <div>
          <Title>Pattern 1</Title>
          <Content>We will see later what to do with the pattern</Content>
        </div>
      </div>
      <div v-if="hide(7)" class="animate" v-bind:class="getStepContainerClass(7)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(7)" /></div>
        <div>
          <Title>Format Error Correction</Title>
          <Content>🤷 No se 🤷</Content>
          <Content>Mask and Error Correction data appear twice, to increase readability.</Content>
        </div>
      </div>
      <div v-if="hide(8)" class="animate" v-bind:class="getStepContainerClass(8)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(8)" /></div>
        <div>
          <Title>Data</Title>
          <Content>Data + Error Correction Data</Content>
        </div>
      </div>
      <div v-if="hide(9)" class="animate" v-bind:class="getStepContainerClass(9)">
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(9)" /></div>
        <div>
          <Title>Overview</Title>
        </div>
      </div>
      <div v-if="hide(10)" v-bind:class="getStepContainerClass(10)">
        <div class="text-center small-qr-code flex justify-around">
          <div><QrCode v-bind:cases="cases" /></div>
          <div class="text-3xl flex items-center font-bold text-green-500">XOR</div>
          <div><QrCode v-bind:cases="cases" v-bind:addClass="mask" /></div>
        </div>
        <div>
          <Title>Decode</Title>
          <Content>XOR between Qr Code and the Mask used</Content>
        </div>
        <div class="text-center"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(10)" /></div>
      </div>
      <div v-if="hide(11)" v-bind:class="getStepContainerClass(11)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(11)" /></div>
        <div>
          <Title>Encoding</Title>
          <Content><a target="_blank" ref="noreferrer noopener" href="https://en.wikipedia.org/wiki/QR_code#/media/File:QR_Character_Placement.svg">Read Data</a></Content>
          <Content>
            <div class="grid grid-cols-2 gap-x-2">
              <div class="justify-self-end">
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="1" />
              </div>
              <div class="justify-self-start">Numeric</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="1" />
                <SmallCase v-bind:value="0" />
              </div>
              <div class="justify-self-start">Alphanumeric</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="1" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
              </div>
              <div class="justify-self-start">Byte</div>
              <div class="justify-self-end">
                <SmallCase v-bind:value="1" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
                <SmallCase v-bind:value="0" />
              </div>
              <div class="justify-self-start">kanji</div>
            </div>
          </Content>
          <Spoiler>Byte</Spoiler>
        </div>
      </div>
      <div v-if="hide(12)" v-bind:class="getStepContainerClass(12)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(12)" /></div>
        <div>
          <Title>Encoding</Title>
          <Spoiler>00001001 => 9 chars</Spoiler>
        </div>
      </div>
      <div v-if="hide(13)" v-bind:class="getStepContainerClass(13)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(13)" /></div>
        <div>
          <Title>First Char</Title>
          <Spoiler>01100100 => 100 (d)</Spoiler>
        </div>
      </div>
      <div v-if="hide(14)" v-bind:class="getStepContainerClass(14)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(14)" /></div>
        <div>
          <Title>Second Char</Title>
          <Spoiler>01101100 => 108 (l)</Spoiler>
        </div>
      </div>
      <div v-if="hide(15)" v-bind:class="getStepContainerClass(15)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(15)" /></div>
        <div>
          <Title>Third Char</Title>
          <Spoiler>01110100 => 116 (t)</Spoiler>
        </div>
      </div>
      <div v-if="hide(16)" v-bind:class="getStepContainerClass(16)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(16)" /></div>
        <div>
          <Title>Fourth Char</Title>
          <Spoiler>00100000 => 32 ( )</Spoiler>
        </div>
      </div>
      <div v-if="hide(17)" v-bind:class="getStepContainerClass(17)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(17)" /></div>
        <div>
          <Title>Fifth Char</Title>
          <Spoiler>01110010 => 114 (r)</Spoiler>
        </div>
      </div>
      <div v-if="hide(18)" v-bind:class="getStepContainerClass(18)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(18)" /></div>
        <div>
          <Title>Sixth Char</Title>
          <Spoiler>01101111 => 111 (o)</Spoiler>
        </div>
      </div>
      <div v-if="hide(19)" v-bind:class="getStepContainerClass(19)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(19)" /></div>
        <div>
          <Title>Seventh Char</Title>
          <Spoiler>01100011 => 99 (c)</Spoiler>
        </div>
      </div>
      <div v-if="hide(20)" v-bind:class="getStepContainerClass(20)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(20)" /></div>
        <div>
          <Title>Eighth Char</Title>
          <Spoiler>01101011 => 107 (k)</Spoiler>
        </div>
      </div>
      <div v-if="hide(21)" v-bind:class="getStepContainerClass(21)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(21)" /></div>
        <div>
          <Title>Ninth Char</Title>
          <Spoiler>01110011 => 115 (s)</Spoiler>
        </div>
      </div>
      <div v-if="hide(22)" v-bind:class="getStepContainerClass(22)">
        <div class="text-center hoverable"><QrCode v-bind:cases="cases" v-bind:addClass="getStep(22)" /></div>
        <div>
          <Title>End</Title>
          <Content>0000</Content>
          <Spoiler>dlt rocks</Spoiler>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.empty * {
  height: 0 !important;
}

.animate * {
  transition-property: background-color, border-color, color, fill, stroke;
  transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
  transition-duration: 600ms;
}

.small-qr-code span,
.small-qr-code .case-container div {
  width: 0.5rem;
  height: 0.5rem;
}
.small-qr-code .case-container {
  height: 0.5rem;
}
.hoverable .case2:hover {
  background-color: #8b5cf6;
}
/*.lol::after {*/
/*  background: red;*/
/*  width: 20px;*/
/*  content: "";*/
/*  position: absolute;*/
/*  display: inline-block;*/
/*  height: 2px;*/
/*  top: 50%;*/
/*  left: -50%;*/
/*}*/
/*.lol4::after {*/
/*  width: 20px;*/
/*  content: "";*/
/*  border: solid black;*/
/*  border-width: 0 3px 3px 0;*/
/*  display: inline-block;*/
/*  !*padding: 8px;*!*/
/*  !*transform: rotate(-45deg);*!*/
/*}*/
</style>
