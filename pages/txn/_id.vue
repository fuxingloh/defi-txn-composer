<template>
  <div class="container mx-auto py-8">
    <div>
      <h1 class="text-xl font-bold">TXID: {{ data.txid }}</h1>
      <h2 class="text-xl font-bold">HASH: {{ data.hash }}</h2>
    </div>

    <div class="mt-6">
      <h1 class="mb-3 text-3xl font-bold">Inputs</h1>
      <div v-for="input in data.vin" class="p-4 bg-gray-200 rounded mb-8">
        <pre class="text-sm">{{ input }}</pre>
      </div>
    </div>

    <div class="mt-6">
      <h1 class="mb-3 text-3xl font-bold">Ouputs</h1>
      <div v-for="output in data.vout" class="p-4 bg-gray-200 rounded mb-8">
        <pre class="text-sm">{{ output }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import {SmartBuffer} from 'smart-buffer'
import {toOPCodes} from "@defichain/jellyfish-transaction/dist/script";

function decode(hex) {
  const buffer = SmartBuffer.fromBuffer(Buffer.from(hex, 'hex'))
  return toOPCodes(buffer)
}

export default {
  data() {
    return {
      raw: '',
      serialized: null
    }
  },
  async asyncData({params}) {
    const id = params.id
    const res = await fetch(`https://mainnet-api.defichain.io/api/DFI/mainnet/tx/${id}/decoderaw`)
    const data = await res.json()
    return {
      data: {
        ...data,
        vin: data.vin.map(vin => {
          return {
            decoded: decode(vin.scriptSig.hex),
            ...vin
          }
        }),
        vout: data.vout.map(vout => {
          return {
            decoded: decode(vout.scriptPubKey.hex),
            ...vout
          }
        })
      }
    }
  }
}
</script>

<style>
.container {
  max-width: 1200px;
}
</style>
