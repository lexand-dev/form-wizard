<template>
  <div class="max-w-[1360px] mx-auto p-4 font-sans">
    <div class="flex flex-col md:flex-row gap-x-8 justify-between">
      <!-- Formulario -->
      <div class="flex flex-col order-2 md:order-1 md:w-1/4">
        <!-- Formulario -->
        <div v-if="currentStep <= steps.length">
          <h2 class="text-xl font-bold mb-4">{{ steps[currentStep - 1] }}</h2>
          <div v-if="currentStep === 1">
            <label class="block mb-2">Banco</label>
            <input v-model="form.banco" class="border p-2 w-full mb-4" />
            <label class="block mb-2">IBAN</label>
            <input v-model="form.iban" class="border p-2 w-full" />
          </div>

          <div v-if="currentStep === 2">
            <label class="block mb-2">Nombre Arrendador</label>
            <input v-model="form.arrendador" class="border p-2 w-full mb-4" />
            <label class="block mb-2">DNI/NIF Arrendador</label>
            <input v-model="form.dniArrendador" class="border p-2 w-full" />
          </div>

          <div v-if="currentStep === 3">
            <label class="block mb-2">Nombre Arrendatario</label>
            <input v-model="form.arrendatario" class="border p-2 w-full mb-4" />
            <label class="block mb-2">DNI/NIF Arrendatario</label>
            <input v-model="form.dniArrendatario" class="border p-2 w-full" />
          </div>

          <div class="flex justify-between mt-6">
            <button
              class="bg-gray-400 text-white px-4 py-2 rounded"
              @click="prevStep"
              :disabled="currentStep === 1"
            >
              Atrás
            </button>
            <button class="bg-blue-500 text-white px-4 py-2 rounded" @click="nextStep">
              {{ currentStep === steps.length ? 'Finalizar' : 'Siguiente' }}
            </button>
          </div>
        </div>
      </div>

      <!-- Vista Previa -->
      <div class="flex-1 hidden md:block order-2">
        <h2 class="text-xl font-bold mb-4">Vista Previa del Contrato</h2>
        <div class="border p-4 bg-white whitespace-pre-wrap shadow-md rounded-md">
          {{ contratoTexto }}
        </div>
        <button class="mt-4 bg-green-500 text-white px-4 py-2 rounded" @click="descargarWord">
          Descargar Word
        </button>
      </div>

      <!-- Barra de Progreso -->
      <div class="md:w-1/4 order-1 md:order-3 mt-8 md:mt-0">
        <div class="text-center text-sm font-semibold mb-2">
          Progreso: {{ currentStep }} de {{ steps.length + 1 }}
        </div>
        <div class="bg-[#191E55] rounded-xl h-6 mb-6 flex items-center">
          <div
            class="bg-gray-200/60 h-4 rounded-xl transition-all text-xs text-white pl-3 m-1 font-semibold flex items-center"
            :style="{ width: progress + '%' }"
          >
            {{ progressText }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'
import { Document, Packer, Paragraph, TextRun } from 'docx'
import { saveAs } from 'file-saver'

const steps = ['Datos Bancarios', 'Datos Arrendador', 'Datos Arrendatario']
const currentStep = ref(1)
const form = reactive({
  banco: '',
  iban: '',
  arrendador: '',
  dniArrendador: '',
  arrendatario: '',
  dniArrendatario: '',
})

const progress = computed(() => Math.round((currentStep.value / (steps.length + 1)) * 100))
const progressText = computed(() => `${progress.value}%`)

const nextStep = () => {
  if (currentStep.value < steps.length + 1) currentStep.value++
}
const prevStep = () => {
  if (currentStep.value > 1) currentStep.value--
}

const contratoTexto = computed(
  () => `
CONTRATO DE ARRENDAMIENTO DE VIVIENDA

Banco: ${form.banco}
IBAN: ${form.iban}

Arrendador: ${form.arrendador} (DNI: ${form.dniArrendador})
Arrendatario: ${form.arrendatario} (DNI: ${form.dniArrendatario})
`,
)

const descargarWord = () => {
  const doc = new Document({
    sections: [
      {
        children: [
          new Paragraph({
            children: [new TextRun(contratoTexto.value)],
          }),
        ],
      },
    ],
  })

  Packer.toBlob(doc).then((blob) => {
    saveAs(blob, 'contrato.docx')
  })
}
</script>

<style>
body {
  background-color: #f8f9fa;
}
</style>
