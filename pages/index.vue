<template>
  <v-app>
    <v-container>
      <v-card class="mt-10">
        <v-select
          v-model="vendedorSelecionado"
          :items="vendedores"
          item-title="nome"
          item-value="id"
          label="Selecione um vendedor"
        ></v-select>
        
        <v-text-field
          v-model="faturamento"
          label="Faturamento"
          type="text"
          hide-details
        ></v-text-field>
  
        <v-btn @click="adicionarComissao" class="mt-10 mb-5 ml-5">Calcular Comissão</v-btn>
        
        <v-data-table
          v-if="tableData.length > 0"
          :headers="headers"
          :items="tableData"
          class="elevation-1"
        ></v-data-table>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>

export default {
  data() {
    return {
      vendedores: [
        { id: 1, nome: 'Anabel' },
        { id: 2, nome: 'Julius' },
        { id: 3, nome: 'Paulo' },
        { id: 4, nome: 'Ana' }
      ],
      faixas: [
        { min: 0.01, max: 1000.00, percentual: 5 },
        { min: 1000.01, max: 10000.00, percentual: 10 },
        { min: 10000.01, max: 100000.00, percentual: 20 },
        { min: 100000.01, max: 1000000.00, percentual: 30 }
      ],
      headers: [
        { title: 'Vendedor', value: 'vendedor' },
        { title: 'Faturamento', value: 'faturamento' },
        { title: 'Comissão', value: 'comissao' }
      ],
      faturamento: 0,
      vendedorSelecionado: null,
      tableData: [],
    };
  },

  methods: {
    calcularComissao(valor) {
      for (const faixa of this.faixas) {
        if (valor >= faixa.min && valor <= faixa.max) {
          return (valor * faixa.percentual) / 100;
        }
      }
      return 0;
    },

    adicionarComissao() {
      const vendedor = this.vendedores.find(vendedores => vendedores.id === this.vendedorSelecionado);
      if (vendedor && this.faturamento > 0) {
        const comissao = this.calcularComissao(this.faturamento);

        this.tableData = [{
          vendedor: vendedor.nome,
          faturamento: this.faturamento,
          comissao: comissao
        }];
        this.resetarFormulario();
      }
    },

    resetarFormulario() {
      this.faturamento = 0;
      this.vendedorSelecionado = null;
    }
  },
};
</script>

<style>

</style>