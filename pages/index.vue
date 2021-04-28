<template>
  <div class="container mx-auto py-8">
    <h1 class="text-2xl font-bold">Raw Txn V4</h1>
    <textarea class="mt-3 h-64 border rounded w-full" @change="onRaw" v-model="raw">
    </textarea>

    <h1 class="mt-6 text-2xl font-bold">Deserialized</h1>
    <pre class="mt-3 text-sm bg-gray-100 p-4 border rounded w-full">{{ serialized }}</pre>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import {SmartBuffer} from 'smart-buffer'
import {CTransactionSegWit, CTransaction} from "@defichain/jellyfish-transaction";
//
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
      if (this.raw.startsWith('040000000001')) {
        const tx = new CTransactionSegWit(buffer)
        this.serialized = tx.toObject()
      } else {
        const tx = new CTransaction(buffer)
        this.serialized = tx.toObject()
      }
    }
  }
})
</script>

<style>
</style>
