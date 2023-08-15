<template>
  <v-container class="fill-height">
    <v-responsive class="fill-height">
      <v-card>
        <v-tabs v-model="tab" bg-color="primary">
          <v-tab value="one">Geral</v-tab>
          <v-tab value="two">Detalhamento</v-tab>
        </v-tabs>

        <v-card-text>
          <v-window v-model="tab">
            <v-window-item value="one">
              <v-btn
                class="text-none text-subtitle-1"
                color="#5865f2"
                size="small"
                variant="flat"
                style="color: white"
                @click="dialog = true"
              >
                Adicionar Operações
              </v-btn>
              <v-table>
                <thead>
                  <tr>
                    <th class="text-left">Name</th>
                    <th class="text-left">Usuários</th>
                    <th class="text-left">Ações</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, index) in dados" :key="item.name">
                    <td>{{ item.name }}</td>
                    <td>{{ item.usuarios.length }}</td>
                    <td><v-btn color="red" @click="removerOperacao(index)">Remover</v-btn></td>
                  </tr>
                </tbody>
              </v-table>
            </v-window-item>

            <v-window-item value="two">
              <v-expansion-panels>
                <v-expansion-panel v-for="(operacao, index) in dados" :key="operacao.name" :title="operacao.name">
                  <v-expansion-panel-text>
                    <v-text-field label="Prefixo" variant="underlined" v-model="dados[index].prefixo" />
                    <v-text-field label="Quantidade" variant="underlined" v-model="dados[index].quantidade" />
                    <v-text-field label="Senha" variant="underlined" v-model="dados[index].senha" />

                    <v-btn
                      class="text-none text-subtitle-1"
                      color="#5865f2"
                      size="small"
                      variant="flat"
                      style="color: white"
                      @click="criarUsuarios(index)"
                    >
                      Adicionar Usuários
                    </v-btn>

                    <v-row no-gutters>
                      <v-col cols="6"
                        ><v-checkbox label="Faz PIX" v-model="dados[index].configuracoes" value="faz_pix"></v-checkbox
                      ></v-col>
                      <v-col cols="6"
                        ><v-checkbox
                          label="Faz Dinheiro"
                          v-model="dados[index].configuracoes"
                          value="faz_dinheiro"
                        ></v-checkbox
                      ></v-col>
                      <v-col cols="6"
                        ><v-checkbox
                          label="Faz Crédito"
                          v-model="dados[index].configuracoes"
                          value="faz_credito"
                        ></v-checkbox
                      ></v-col>
                      <v-col cols="6"
                        ><v-checkbox
                          label="Faz Débito"
                          v-model="dados[index].configuracoes"
                          value="faz_debito"
                        ></v-checkbox
                      ></v-col>
                      <v-col cols="6"
                        ><v-checkbox
                          label="Faz Consumo"
                          v-model="dados[index].configuracoes"
                          value="faz_consumo"
                        ></v-checkbox
                      ></v-col>
                      <v-col cols="6"
                        ><v-checkbox
                          label="Faz Recarga"
                          v-model="dados[index].configuracoes"
                          value="faz_recarga"
                        ></v-checkbox
                      ></v-col>
                    </v-row>

                    <v-expansion-panels>
                      <v-expansion-panel
                        v-for="(usuario, id_u) in dados[index].usuarios"
                        :key="usuario.id_u"
                        :title="usuario.label"
                      >
                        <v-expansion-panel-text>
                          <v-row no-gutters>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz PIX"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_pix"
                              ></v-checkbox
                            ></v-col>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz Dinheiro"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_dinheiro"
                              ></v-checkbox
                            ></v-col>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz Crédito"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_credito"
                              ></v-checkbox
                            ></v-col>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz Débito"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_debito"
                              ></v-checkbox
                            ></v-col>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz Consumo"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_consumo"
                              ></v-checkbox
                            ></v-col>
                            <v-col cols="6"
                              ><v-checkbox
                                label="Faz Recarga"
                                v-model="dados[index].usuarios[id_u].configuracoes"
                                value="faz_recarga"
                              ></v-checkbox
                            ></v-col>
                          </v-row>
                        </v-expansion-panel-text>
                      </v-expansion-panel>
                    </v-expansion-panels>
                  </v-expansion-panel-text>
                </v-expansion-panel>
              </v-expansion-panels>
            </v-window-item>
          </v-window>
        </v-card-text>
      </v-card>
    </v-responsive>

    <v-dialog v-model="dialog" width="auto">
      <v-card>
        <v-card-title>Quantas operações quer adicionar?</v-card-title>
        <v-card-text>
          <v-text-field label="Prefixo" variant="underlined" v-model="prefixo" />
          <v-text-field label="Quantidade" variant="underlined" v-model="qtd" />
        </v-card-text>
        <v-card-title>Configurações Iniciais Usuários</v-card-title>
        <v-card-text>
          <v-row no-gutters>
            <v-col cols="6"><v-checkbox label="Faz PIX" v-model="configuracoes" value="faz_pix"></v-checkbox></v-col>
            <v-col cols="6"
              ><v-checkbox label="Faz Dinheiro" v-model="configuracoes" value="faz_dinheiro"></v-checkbox
            ></v-col>
            <v-col cols="6"
              ><v-checkbox label="Faz Crédito" v-model="configuracoes" value="faz_credito"></v-checkbox
            ></v-col>
            <v-col cols="6"
              ><v-checkbox label="Faz Débito" v-model="configuracoes" value="faz_debito"></v-checkbox
            ></v-col>
            <v-col cols="6"
              ><v-checkbox label="Faz Consumo" v-model="configuracoes" value="faz_consumo"></v-checkbox
            ></v-col>
            <v-col cols="6"
              ><v-checkbox label="Faz Recarga" v-model="configuracoes" value="faz_recarga"></v-checkbox
            ></v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" variant="text" @click="criarOperacoes(qtd, prefixo)"> Criar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup>
import { ref } from 'vue';

const dados = ref([]);
const dialog = ref(false);
const prefixo = ref('Operação');
const qtd = ref(1);
const ultimoIndex = ref(0);
const tab = ref(0);
const configuracoes = ref([]);

function criarOperacoes(qtd, prefixo) {
  for (let index = 0; index < qtd; index++) {
    ultimoIndex.value = ultimoIndex.value + 1;
    dados.value.push({
      name: `${prefixo} ${ultimoIndex.value}`,
      usuarios: [],
      configuracoes: configuracoes.value,
      prefixo: 'Caixa',
      quantidade: 0,
      senha: 'imply1111',
    });
  }

  dialog.value = false;
}

function removerOperacao(index) {
  dados.value.splice(index, 1);
}

function criarUsuarios(index) {
  const { prefixo, senha, quantidade, configuracoes } = dados.value[index];

  for (let id_u = 0; id_u < quantidade; id_u++) {
    dados.value[index].usuarios.push({
      usuario: `${prefixo.toLowerCase()}_${index}_${id_u}`,
      label: `${prefixo} ${id_u + 1}`,
      senha,
      configuracoes,
    });
  }
}
</script>
