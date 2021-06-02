<template>
  <div class="container mx-auto py-8">
    <h1 class="text-2xl font-bold">Raw DfTx</h1>
    <textarea class="mt-3 h-64 border rounded w-full" @change="onRaw" v-model="raw">
    </textarea>

    <h1 class="mt-6 text-2xl font-bold">Deserialized</h1>
    <pre class="mt-3 text-sm bg-gray-100 p-4 border rounded w-full">{{ serialized }}</pre>
  </div>
</template>

<script>
import Vue from 'vue'
import {SmartBuffer} from 'smart-buffer'
import {OP_CODES} from "@defichain/jellyfish-transaction/dist/script";

export default Vue.extend({
  data() {
    return {
      raw: '',
      serialized: null
    }
  },
  methods: {
    onRaw() {
      const buffer = SmartBuffer.fromBuffer(Buffer.from(this.raw, 'hex'))
      this.serialized = OP_CODES.fromBuffer(buffer)
    }
  }
})
</script>

<style>
</style>
