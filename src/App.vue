<template>
  <v-app>
    <v-main>
      <v-container class="mt-16">
        <v-row class="mb-6">
          <v-col cols="12">
            <h1 class="calculator-title">Investment Calculator</h1>
          </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" md="6">
              <v-row>
                <v-alert            
                  color="error"
                  icon="$error"
                  title="Usa solo números."
                  text="Todos los campos deben tener información."
                  v-if="validationError"
                  >
                </v-alert>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-text-field :prefix="currency" type="number" v-model.number="amount" label="Inversión Inicial" variant="outlined" density="compact" @input="onChange($event)">
                    <!-- <template v-slot:append-inner>
                      <v-tooltip
                        location="bottom"
                        :open-on-hover="true"
                        open-on-click
                      >
                        <template v-slot:activator="{ props }">
                          <v-icon
                            v-bind="props"
                            icon="mdi-help-circle-outline"
                          ></v-icon>
                        </template>
                        <span>Monto inicial disponible para invertir</span>
                      </v-tooltip>
                    </template> -->
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-text-field :prefix="currency" type="number" v-model.number="payment" label="Contribuciones" variant="outlined" density="compact" @input="onChange($event)">
                    <!-- <template v-slot:append-inner>
                      <v-tooltip
                        location="bottom"
                        :open-on-hover="true"
                        open-on-click
                      >
                        <template v-slot:activator="{ props }">
                          <v-icon
                            v-bind="props"
                            icon="mdi-help-circle-outline"
                          ></v-icon>
                        </template>
                        <span>Monto de los aportes recurrentes</span>
                      </v-tooltip>
                    </template> -->
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-select
                    v-model="frequencyOption"
                    :menu-props="{ 'max-height': 500 }"
                    :hint="`${frequencyOption.state}, ${frequencyOption.valorFreq}`"
                    :items="itemsFreq"
                    item-title="state"
                    item-value="valorFreq"
                    label="Frecuencia"
                    return-object
                    variant="outlined"
                    density="compact" 
                  ></v-select>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-text-field :prefix="percent" type="number" v-model.number="retorno" label="Retorno Esperado" variant="outlined" density="compact" @input="onChange($event)">
                    <!-- <template v-slot:append-inner>
                      <v-tooltip
                        location="bottom"
                        :open-on-hover="true"
                        open-on-click
                      >
                        <template v-slot:activator="{ props }">
                          <v-icon
                            v-bind="props"
                            icon="mdi-help-circle-outline"
                          ></v-icon>
                        </template>
                        <span>Ganancia anual esperada en porcentaje</span>
                      </v-tooltip>
                    </template> -->
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-select
                    v-model="compoundingOption"
                    :menu-props="{ 'max-height': 500 }"
                    :hint="`${compoundingOption.state}, ${compoundingOption.valorComp}`"
                    :items="itemsComp"
                    item-title="state"
                    item-value="valorComp"
                    label="Compounding"
                    return-object
                    variant="outlined"
                    density="compact" 
                  ></v-select>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="col-style">
                  <v-text-field type="number" v-model.number="years" label="Años" variant="outlined" density="compact" @input="onChange($event)">
                    <!-- <template v-slot:append-inner>
                      <v-tooltip
                        location="bottom"
                        :open-on-hover="true"
                        open-on-click
                      >
                        <template v-slot:activator="{ props }">
                          <v-icon
                            v-bind="props"
                            icon="mdi-help-circle-outline"
                          ></v-icon>
                        </template>
                        <span>Período de tiempo total de la inversión</span>
                      </v-tooltip> 
                    </template> -->
                  </v-text-field>
                </v-col>
              </v-row>
            </v-col>
          <!--<v-row>
          <v-col cols="6">
          <v-btn color="primary" @click="calculate">Calculate</v-btn>
          </v-col>
          </v-row>-->
          <v-col cols="12" md="6">
            <v-row class="px-md-10">
              <v-col cols="12" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(result) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="monthly-payment-title">TOTAL</h1>
              </v-col>
              <v-col cols="12" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(inversion) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="monthly-payment-title">INVERSIÓN TOTAL</h1>
              </v-col>
              <v-col cols="12" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(contribuciones) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="monthly-payment-title">CONTRIBUCIONES</h1>
              </v-col>
              <v-col cols="12" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(interes) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="monthly-payment-title">GANANCIA</h1>
              </v-col>
            </v-row>
          </v-col>
        <hr class="my-12">
        <v-row>
          <v-col cols="12">
            <h1 class="titlepie"> Crecimiento Anual </h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="barchartdiv" style="width: 100%; height: 500px;"></div>
          </v-col>
        </v-row>
        </v-row>
        <hr class="my-12">
        <v-row class="mt-12 d-flex justify-center">
          <v-col cols="12" md="6">
            <div>
              <h1 class="titlepie"> Primer Año </h1>
              <v-divider class="monthly-payment-line"></v-divider>
              <div id="chartdiv" style="width: 100%; height: 400px;"></div>
            </div>
          </v-col>
          <v-col cols="12" class="d-lg-none my-12">
            <hr>
          </v-col>
          <v-col cols="12" md="6">
            <h1 class="titlepie"> Balance Final </h1>
            <v-divider class="monthly-payment-line"></v-divider>
            <div id="chartdivlast" style="width: 100%; height: 400px;"></div>
          </v-col>
        </v-row>
        <hr class="my-12">
        <v-row>
          <v-col cols="12">
            <v-card>
              <v-tabs class="tabtext" v-model="tab" bg-color="#b54914">
                <v-tab value="anualizado">Crecimiento Anualizado</v-tab>
                <v-tab value="mensual">Tabla {{ frequencyOption.state }}</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab"> 
                  <v-window-item value="anualizado">
                    <v-table fixed-header  class="w-100"  height="580px">
                      <thead>
                        <tr>
                          <th class="text-left">Año</th>
                          <th class="text-left">Principal</th>
                          <th class="text-left">Ganancia</th>
                          <th class="text-left">Ganancia Total</th>
                          <th class="text-left">Balance Final</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in tablaAnualizada" :key="item.name">
                          <td>{{ item.year }}</td>
                          <td>{{ formattedNumber(item.balance) }}</td>
                          <td>{{ formattedNumber(item.total) }}</td>
                          <td>{{ formattedNumber(item.ganancia) }}</td>
                          <td>{{ formattedNumber(item.finalBalance) }}</td>
                        </tr>
                      </tbody>
                    </v-table>
                  </v-window-item>
                  <v-window-item value="mensual">
                    <v-table fixed-header  class="w-100"  height="580px">
                      <thead>
                        <tr>
                          <th class="text-left">{{ frequencyOption.freq }}</th>
                          <th class="text-left">Principal</th>
                          <th class="text-left">Ganancia</th>
                          <th class="text-left">Ganancia Total</th>
                          <th class="text-left">Balance Final</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in tablaMensual" :key="item.name">
                          <td>{{ item.mes }}</td>
                          <td>{{ formattedNumber(item.balance) }}</td>
                          <td>{{ formattedNumber(item.ganancia) }}</td>
                          <td>{{ formattedNumber(item.total) }}</td>
                          <td>{{ formattedNumber(item.finalBalance) }}</td>
                        </tr>
                      </tbody>
                    </v-table>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>

import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
am4core.useTheme(am4themes_animated);

export default { 
  name: 'App',
  data() {
    return {
      percent: "%",
      currency: "$",
      frequency: 0,
      compounding: 0,          
      frequencyOption: { state: 'Mensual', valorFreq: 12, freq: 'Mes' },
        itemsFreq: [
          { state: 'Anual', valorFreq: 1, freq: 'Año' },
          { state: 'Semestral', valorFreq: 2, freq: 'Semestre'},
          { state: 'Trimestral', valorFreq: 4, freq: 'Trimestre' },
          { state: 'Mensual', valorFreq: 12, freq: 'Mes' },
          { state: 'Quincenal', valorFreq: 26, freq: 'Quincena' },
          { state: 'Semanal', valorFreq: 52, freq: 'Semana' },
          { state: 'Diario', valorFreq: 365, freq: 'Dia' },
        ],
      compoundingOption: { state: 'Anual', valorComp: 1 },
        itemsComp: [
          { state: 'Anual', valorComp: 1 },
          { state: 'Semestral', valorComp: 2 },
          { state: 'Trimestral', valorComp: 4 },
          { state: 'Mensual', valorComp: 12 },
          { state: 'Quincenal', valorComp: 26 },
          { state: 'Semanal', valorComp: 52 },
          { state: 'Diario', valorComp: 365 },
        ],
      tab: null,
      piechart: null,
      amount: 0,
      payment: 0,
      retorno: 0,
      years: 0,
      result: 0,
      contribuciones: 0,
      contribucionpie: 0,
      interes: 0,
      inversion: 0,
      validationError: false,
      balanceInicial: 0,
      balancePrincipal: 0,
      balanceFinal: 0,
      totalTabla: 0,
      interesTotal: 0,
      interesTabla: 0,
      gananciaAnualizada: 0,
      gananciaAnualizadaLast: 0,
      interesTablaFirst: 0,
      tablaAnual: [],
      tablaMensual: [],
      tablaAnualizada: [],      
      chartData: [],
      chartBarData: [],
    };
  },
  watch: {
    frequencyOption: function () {
      this.onChange();
    },
    compoundingOption: function () {
      this.onChange();
    },
  },
  methods: {
    onChange() {
      if (
        this.amount !== '' &&
        this.payment !== '' &&
        this.retorno !== '' &&
        this.years > 0
      ) {
        this.validationError = false
        this.calculate();
      } else {
        this.validationError = false
      }
    },
    calculate() {
      const currentDate = new Date();
      let actYear = currentDate.getFullYear();
      const frequency = parseFloat(this.frequencyOption.valorFreq);
      const compounding = parseFloat(this.compoundingOption.valorComp);
      const retornoFixed = this.retorno/100;
      const rate = Math.pow((1+retornoFixed/compounding), (compounding/frequency))-1;
      const nper = frequency*this.years;
      const contribucionpie = this.payment*frequency;
      this.contribuciones = this.payment*nper;
      const resultRaw = rate == 0 ? 0 : (this.amount * Math.pow((1+rate), nper) + this.payment*((Math.pow((1+rate), nper)-1)/rate));
      this.result = resultRaw.toFixed(2).toLocaleString();
      this.interes = resultRaw == 0 ? 0 : (resultRaw - this.amount - this.contribuciones).toFixed(2).toLocaleString();
      this.inversion = this.contribuciones + this.amount;
      this.balancePrincipal = this.amount + this.payment;
      let balanceTotal = this.balancePrincipal;

      //Calcular Tabla Anual
      const yearsQuantity = this.years;
      let balanceInicial = this.amount;
      let totalTabla = this.amount * Math.pow(1+Math.pow((1+retornoFixed/compounding), (compounding/1))-1, 1);
      let interesTabla = totalTabla - balanceInicial;
      let balanceFinal = totalTabla + (this.payment*nper/this.years/1);
      this.tablaAnual = [];
      this.tablaAnual.push({ años: 1, balance: balanceInicial.toFixed(2), ganancia: interesTabla.toFixed(2), total: totalTabla.toFixed(2), finalBalance: balanceFinal.toFixed(2) });

      for (let index = 2; index <= yearsQuantity; index++) {
        balanceInicial = balanceFinal;
        totalTabla = balanceInicial * Math.pow(1+Math.pow((1+retornoFixed/compounding), (compounding/1))-1, 1);
        interesTabla = totalTabla - balanceInicial;
        balanceFinal = totalTabla + (this.payment*nper/this.years/1);
        this.tablaAnual.push({ años: index, balance: balanceInicial.toFixed(2), ganancia: interesTabla.toFixed(2), total: totalTabla.toFixed(2), finalBalance: balanceFinal.toFixed(2) });
        //PIE LAST
        if (index == yearsQuantity) {
          this.chartLast.data = [{
            "categoria": "Ganancia Total",
            "valor": interesTabla,
            "color": "#4ed8c0",
          },
          {
            "categoria": "Inversión Inicial",
            "valor": this.amount,
            "color": "#7c6af7",
          },
          {
            "categoria": "Contribuciones",
            "valor": this.payment*nper,
            "color": "#E6AE3F",
          }];
        }
      }

      //Calcular Tabla por Frecuencia
      const monthQuantity = this.years*frequency;
      let frequencyCounter = 1;
      let yearsCounter = 0;
      let interesAnualizado = 0;
      let graficaStartInit = 0;
      let interesTotal = 0;
      
      let interesTablaFirst = 0;
      let acumuladoGanancia = 0;
      
      this.tablaMensual = [];
      this.tablaAnualizada = [];
      this.chartBarData = [];

      for (let index = 1; index <= monthQuantity; index++) {
        if(index == 1) {
          balanceInicial = this.amount;
          totalTabla = this.amount * Math.pow(1+Math.pow((1+retornoFixed/compounding), (compounding/frequency))-1, 1);
          interesTabla = totalTabla - balanceInicial;
          interesTotal = interesTabla;
          balanceTotal = balanceInicial+this.payment;
        } else {
          balanceTotal = balanceTotal + this.payment;
          balanceInicial = balanceFinal;
          totalTabla = balanceInicial * Math.pow(1+Math.pow((1+retornoFixed/compounding), (compounding/frequency))-1, 1);
          interesTabla = totalTabla - balanceInicial;
          interesTotal = interesTotal+interesTabla;          
        }
        balanceFinal = totalTabla + (this.payment*nper/this.years/frequency);

        acumuladoGanancia += interesTabla;  

        this.tablaMensual.push({ mes: index, balance: balanceTotal.toFixed(2), ganancia: interesTabla.toFixed(2), total: interesTotal.toFixed(2), finalBalance: balanceFinal.toFixed(2) });
        if(frequencyCounter === frequency) {
          yearsCounter++;
          this.tablaAnualizada.push({ year: yearsCounter, balance: balanceTotal.toFixed(2), ganancia: interesTotal.toFixed(2), total: acumuladoGanancia.toFixed(2), finalBalance: balanceFinal.toFixed(2) })
          //BARRA CALC
          this.chartBarData.push({
            "date": actYear.toString(),
            "principal": parseFloat(balanceTotal.toFixed(2)),
            "ganancia": parseFloat(acumuladoGanancia.toFixed(2)),
            "ganancia_total": parseFloat(interesTotal.toFixed(2)),
            "total": balanceFinal.toLocaleString('en-US', {
              style: 'currency',
              currency: 'USD', // Cambia a tu moneda deseada (ejemplo: 'EUR', 'JPY', 'GBP', etc.)
              minimumFractionDigits: 2,
              maximumFractionDigits: 2,
            }),
          });
          acumuladoGanancia = 0;
          actYear++;
          //BARRA CALC
          frequencyCounter = 0;
        }
        if (index > (monthQuantity-frequency)) {
          interesAnualizado += interesTabla;
        }
        frequencyCounter++;
      }

      this.chartBar.data = [...this.chartBarData];

      let firstValue = this.tablaAnualizada[0].ganancia;
      interesTablaFirst = firstValue;
      console.log(interesAnualizado);
      graficaStartInit = interesTotal;

      this.chart.data = [{
          "categoria": "Ganancia Total",
          "valor": parseFloat(interesTablaFirst),
          "color": "#4ed8c0",
        },
        {
          "categoria": "Inversión Inicial",
          "valor": this.amount,
          "color": "#7c6af7",
        },
        {
          "categoria": "Contribuciones",
          "valor": contribucionpie,
          "color": "#f06fba",
        }];

      this.chartLast.data = [{
          "categoria": "Ganancia Total",
          "valor": graficaStartInit,
          "color": "#4ed8c0",
        },
        {
          "categoria": "Inversión Inicial",
          "valor": this.amount,
          "color": "#7c6af7",
        },
        {
          "categoria": "Contribuciones",
          "valor": this.payment*nper,
          "color": "#f06fba",
        }];
    },

    formattedNumber(number) {
      return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  
  mounted() {
      this.chart = am4core.create("chartdiv", am4charts.PieChart);
      if (this.chart.logo) {
        this.chart.logo.dispose()
      }
      this.chart.background.fill = am4core.color("#0f0f18");
      this.chart.background.fillOpacity = 1;
      this.chart.data = [{
          "categoria": "Ganancia Total",
          "valor": 0,
          "color": "#4ed8c0",
        },
        {
          "categoria": "Inversión Inicial",
          "valor": 0,
          "color": "#7c6af7",
        },
        {
          "categoria": "Contribuciones",
          "valor": 0,
          "color": "#f06fba",
        }];
      let pieSeries = this.chart.series.push(new am4charts.PieSeries());
      pieSeries.dataFields.value = "valor";
      pieSeries.dataFields.category = "categoria";
      pieSeries.slices.template.propertyFields.fill = "color";
      pieSeries.labels.template.text = "{value.percent.formatNumber('#.00')}%";
      pieSeries.slices.template.tooltipText = "[bold, font-size:35px]${valor.formatNumber('#,###.##')}\n[normal]{categoria}[/]";
      pieSeries.slices.template.tooltipX = 0;
      pieSeries.slices.template.tooltipY = 40;
      pieSeries.tooltip.label.textAlign = "middle";
      pieSeries.tooltip.background.filters.clear();
      pieSeries.tooltip.background.pointerLength = 0;
      pieSeries.tooltip.background.strokeOpacity = 0;
      pieSeries.tooltip.label.propertyFields.fill = "color";
      pieSeries.tooltip.background.cornerRadius = 50;
      pieSeries.tooltip.label.minWidth = 150;
      pieSeries.tooltip.label.minHeight = 100;
      pieSeries.tooltip.label.textValign = "middle";
      let hs = pieSeries.slices.template.states.getKey("hover");
      hs.properties.scale = 1.08;
      let as = pieSeries.slices.template.states.getKey("active");
      as.properties.shiftRadius = .1;
      pieSeries.labels.template.fill = am4core.color("#6b6b80");
      pieSeries.ticks.template.stroke = am4core.color("#6b6b80");
      // LEYENDA
      this.chart.legend = new am4charts.Legend();
      this.chart.legend.valueLabels.template.text = "";
      this.chart.legend.labels.template.fill = am4core.color("#eeeef6");
      let marker = this.chart.legend.markers.template.children.getIndex(0);
      marker.cornerRadius(15, 15, 15, 15);
      marker.strokeWidth = 2;
      marker.strokeOpacity = 1;
      // ANIAMCION INICIAL
      pieSeries.hiddenState.properties.opacity = 1;
      pieSeries.hiddenState.properties.endAngle = -90;
      pieSeries.hiddenState.properties.startAngle = -90;
      
      // RESPONSIVE
      this.chart.responsive.enabled = true;
      this.chart.responsive.rules.push({
        relevant: function(target) {
          if (target.pixelWidth <= 500) {
            return true;
          }
          return false;
        },
        state: function(target, stateId) {
          if (target instanceof am4charts.PieSeries) {
            var state = target.states.create(stateId); 
            state.properties.paddingTop = 0;
            state.properties.paddingRight = 0;
            state.properties.paddingBottom = 0;
            state.properties.paddingLeft = 0;
            
            var labelState = target.labels.template.states.create(stateId);
            labelState.properties.disabled = true;

            var tickState = target.ticks.template.states.create(stateId);
            tickState.properties.disabled = true;
            return state;
          }
          return null;
        }
      });
      //ChartPieLastYear
      this.chartLast = am4core.create("chartdivlast", am4charts.PieChart);
      if (this.chartLast.logo) {
        this.chartLast.logo.dispose()
      }
      this.chartLast.background.fill = am4core.color("#0f0f18");
      this.chartLast.background.fillOpacity = 1;
      this.chartLast.data = [{
          "categoria": "Ganancia Total",
          "valor": 0,
          "color": "#4ed8c0",
        },
        {
          "categoria": "Balance",
          "valor": 0,
          "color": "#7c6af7",
        },
        {
          "categoria": "Contribuciones",
          "valor": 0,
          "color": "#f06fba",
        }];
      let pieSeriesLast = this.chartLast.series.push(new am4charts.PieSeries());
      pieSeriesLast.dataFields.value = "valor";
      pieSeriesLast.dataFields.category = "categoria";
      pieSeriesLast.slices.template.tooltipText = "{categoria}\n[bold]${valor}[/]";
      pieSeriesLast.slices.template.propertyFields.fill = "color";
      pieSeriesLast.labels.template.text = "{value.percent.formatNumber('#.00')}%";
      pieSeriesLast.slices.template.tooltipText = "[bold, font-size:35px]${valor.formatNumber('#,###.##')}\n[normal]{categoria}[/]";
      pieSeriesLast.slices.template.tooltipX = 0;
      pieSeriesLast.slices.template.tooltipY = 40;
      pieSeriesLast.tooltip.label.textAlign = "middle";
      pieSeriesLast.tooltip.background.filters.clear();
      pieSeriesLast.tooltip.background.pointerLength = 0;
      pieSeriesLast.tooltip.background.strokeOpacity = 0;
      pieSeriesLast.tooltip.label.propertyFields.fill = "color";
      pieSeriesLast.tooltip.background.cornerRadius = 50;
      pieSeriesLast.tooltip.label.minWidth = 150;
      pieSeriesLast.tooltip.label.minHeight = 100;
      pieSeriesLast.tooltip.label.textValign = "middle";
      let hsLast = pieSeriesLast.slices.template.states.getKey("hover");
      hsLast.properties.scale = 1.08;
      let asLast = pieSeriesLast.slices.template.states.getKey("active");
      asLast.properties.shiftRadius = .1;
      pieSeriesLast.labels.template.fill = am4core.color("#6b6b80");
      pieSeriesLast.ticks.template.stroke = am4core.color("#6b6b80");
      //LEYENDA
      this.chartLast.legend = new am4charts.Legend();
      this.chartLast.legend.valueLabels.template.text = "";
      this.chartLast.legend.labels.template.fill = am4core.color("#eeeef6");
      let markerLast = this.chartLast.legend.markers.template.children.getIndex(0);
      markerLast.cornerRadius(15, 15, 15, 15);
      markerLast.strokeWidth = 2;
      markerLast.strokeOpacity = 1;
      this.chartLast.legend.reverseOrder = true;
      //ANIMACION INICIAL
      pieSeriesLast.hiddenState.properties.opacity = 1;
      pieSeriesLast.hiddenState.properties.endAngle = -90;
      pieSeriesLast.hiddenState.properties.startAngle = -90;

      //BAR CHART
      this.chartBar = am4core.create("barchartdiv", am4charts.XYChart);
      if (this.chartBar.logo) {
        this.chartBar.logo.dispose()
      }
      this.chartBar.background.fill = am4core.color("#0f0f18");
      this.chartBar.background.fillOpacity = 1;
      this.chartBar.data = [{
        "date": "",
        "principal": 0,
        "ganancia": 0,
        "ganancia_total": 0,
        "total": 0,
      }];
      //CREATE AXES
      let categoryAxis = this.chartBar.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis.dataFields.category = "date";
      categoryAxis.renderer.grid.template.disabled = true;
      categoryAxis.renderer.minGridDistance = 50;
      categoryAxis.animationDuration = 0;

      let axisTooltip = categoryAxis.tooltip;
      axisTooltip.background.fill = am4core.color("#000");
      axisTooltip.background.strokeWidth = 0;
      axisTooltip.background.cornerRadius = 5;
      axisTooltip.background.pointerLength = 0;
      axisTooltip.animationDuration = 0;
      axisTooltip.dy = 5;

      //TOTAL
      let categoryAxis2 = this.chartBar.xAxes.push(new am4charts.CategoryAxis());
      categoryAxis2.renderer.opposite = true;
      categoryAxis2.dataFields.category = "date";
      categoryAxis2.renderer.grid.template.disabled = true;
      categoryAxis2.renderer.grid.disabled = true;
      categoryAxis2.renderer.labels.template.disabled = true;
      categoryAxis2.animationDuration = 0;
      categoryAxis2.adapter.add("getTooltipText", () => {
        return "{total}";
      });

      axisTooltip = categoryAxis2.tooltip;
      axisTooltip.background.fill = am4core.color("#000");
      axisTooltip.background.strokeWidth = 0;
      axisTooltip.background.cornerRadius = 5;
      axisTooltip.background.pointerLength = 0;
      axisTooltip.animationDuration = 0;
      axisTooltip.dy = 5;

      categoryAxis.renderer.labels.template.fill = am4core.color("#6b6b80");
      categoryAxis.renderer.grid.template.stroke = am4core.color("#ffffff");
      categoryAxis.renderer.grid.template.strokeOpacity = 0.05;

      let valueAxis = this.chartBar.yAxes.push(new am4charts.ValueAxis());
      valueAxis.numberFormatter.numberFormat = "'$'#,###.##";
      valueAxis.tooltip.disabled = true;
      valueAxis.renderer.labels.template.fill = am4core.color("#6b6b80");
      valueAxis.renderer.grid.template.stroke = am4core.color("#ffffff");
      valueAxis.renderer.grid.template.strokeOpacity = 0.05;

      let valueAxis2 = this.chartBar.yAxes.push(new am4charts.ValueAxis());
      valueAxis2.renderer.opposite = true;
      valueAxis2.renderer.grid.template.disabled = true;
      valueAxis2.renderer.labels.template.disabled = true;
      valueAxis2.tooltip.disabled = true;

      //COLORES
      this.chartBar.colors.list = [
        am4core.color("#7c6af7"),
        am4core.color("#4ed8c0"),
        am4core.color("#f06fba"),
      ];
      //CREATE SERIES
      let series = this.chartBar.series.push(new am4charts.ColumnSeries());
      series.dataFields.valueY = "principal";
      series.dataFields.categoryX = "date";
      series.name = "Principal";
      series.tooltipText = "[#fff]{name}: [bold, #fff]{valueY}[/]";
      series.tooltip.background.filters.clear();
      series.tooltip.animationDuration = 0;
      series.stacked = true;

      // GANANCIA TOTAL (antes: COTRIBUCIONES)
      var series2 = this.chartBar.series.push(new am4charts.ColumnSeries());
      series2.dataFields.valueY = "ganancia_total";
      series2.dataFields.categoryX = "date";
      series2.name = "Ganancia Total";
      series2.tooltipText = "[#000]{name}: [bold, #000]{valueY}[/]";
      series2.tooltip.background.filters.clear();
      series2.stacked = true;
      series2.columns.template.column.cornerRadiusTopLeft = 5;
      series2.columns.template.column.cornerRadiusTopRight = 5;
      series2.tooltip.animationDuration = 0;

      // GANANCIA (nueva serie) 
      let series3 = this.chartBar.series.push(new am4charts.LineSeries());
      series3.dataFields.valueY = "ganancia";
      series3.dataFields.categoryX = "date";
      series3.yAxis = valueAxis2;
      series3.name = "Ganancia";
      series3.strokeWidth = 3;
      series3.tooltipText = "[#000]{name}: [bold, #000]{valueY}[/]";
      series3.strokeWidth = 3;
      series3.tooltip.animationDuration = 0;

      //CURSOR
      this.chartBar.cursor = new am4charts.XYCursor();
      this.chartBar.cursor.behavior = "none";
      this.chartBar.cursor.lineY.disabled = true;
      this.chartBar.cursor.fullWidthLineX = true;
      this.chartBar.cursor.xAxis = categoryAxis;
      this.chartBar.cursor.lineX.strokeOpacity = 0;
      this.chartBar.cursor.lineX.fill = "#000";
      this.chartBar.cursor.lineX.fillOpacity = 0.1;
      this.chartBar.cursor.animationDuration = 0;
      //LEYENDA
      this.chartBar.legend = new am4charts.Legend();
      this.chartBar.legend.labels.template.fill = am4core.color("#eeeef6");
      // BOTON ZOOM-OUT
      this.chartBar.zoomOutButton.align = "left";
      this.chartBar.zoomOutButton.valign = "top";
      this.chartBar.zoomOutButton.background.defaultState.properties.fill = "#000";
      this.chartBar.zoomOutButton.background.defaultState.properties.fillOpacity = .2;
      this.chartBar.zoomOutButton.background.states.getKey("hover").properties.fill = "#000";
      this.chartBar.zoomOutButton.background.states.getKey("down").properties.fill = "#000";
      this.chartBar.zoomOutButton.icon.strokeWidth = 2;

    },  
}
</script>
<style>
  :root {
    --bg: #08080e;
    --bg2: #0f0f18;
    --bg3: #14141e;
    --border: rgba(255,255,255,0.07);
    --border-bright: rgba(255,255,255,0.15);
    --text: #eeeef6;
    --muted: #6b6b80;
    --accent: #7c6af7;
    --accent2: #4ed8c0;
    --accent3: #f06fba;
  }

  body { background: var(--bg) !important; }

  .v-application { background: var(--bg) !important; font-family: 'Syne', sans-serif !important; }

  input {
    font-family: 'Syne', sans-serif !important;
    color: var(--text) !important;
    background-color: transparent !important;
  }

  .v-select .v-select__selection-text { font-family: 'Syne', sans-serif !important; color: var(--text) !important; }

  .monthly-payment-mount {
    font-family: 'Raleway', sans-serif !important;
    font-weight: 800 !important;
    font-size: 2.2rem !important;
    color: var(--text) !important;
    line-height: 1.1 !important;
  }

  .monthly-payment-title {
    font-family: 'Space Mono', monospace !important;
    text-transform: uppercase !important;
    font-weight: 400 !important;
    font-size: 11px !important;
    letter-spacing: 0.12em !important;
    color: var(--muted) !important;
    margin-bottom: 1rem !important;
    margin-top: 4px !important;
  }

  .monthly-payment-line {
    width: 32px !important;
    border-width: 2px !important;
    margin-bottom: 6px !important;
    margin-top: 12px !important;
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    border-color: transparent !important;
    opacity: 1 !important;
  }

  .titlepie {
    font-family: 'Raleway', sans-serif !important;
    text-transform: uppercase !important;
    font-weight: 800 !important;
    font-size: 12px !important;
    letter-spacing: 0.18em !important;
    color: var(--muted) !important;
    margin-bottom: -20px !important;
  }

  .v-card { background: var(--bg2) !important; border: 1px solid var(--border) !important; box-shadow: none !important; }

  .v-tabs { background: var(--bg3) !important; border-bottom: 1px solid var(--border) !important; }

  .v-tab {
    font-family: 'Space Mono', monospace !important;
    font-size: 11px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
  }

  .v-tab--selected { color: var(--accent2) !important; }

  .v-tab__slider {
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    height: 2px !important;
  }

  .v-table { background: var(--bg2) !important; color: var(--text) !important; }

  .v-table thead tr th {
    font-family: 'Space Mono', monospace !important;
    font-size: 10px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
    background: var(--bg3) !important;
    border-bottom: 1px solid var(--border) !important;
  }

  .v-table tbody tr td {
    font-family: 'Syne', sans-serif !important;
    font-size: 13px !important;
    color: var(--text) !important;
    border-bottom: 1px solid var(--border) !important;
  }

  .v-table tbody tr:hover td { background: rgba(124,106,247,0.08) !important; }

  .v-table table tr:hover { background-color: rgba(124,106,247,0.08) !important; color: var(--text) !important; }

  .calculator-investment { line-height: 0.5em; }

  .calculator-title {
    font-family: 'Raleway', sans-serif !important;
    font-weight: 800 !important;
    font-size: 2rem !important;
    color: var(--text) !important;
    letter-spacing: 0.02em !important;
  }

  .col-style { padding-bottom: 0 !important; padding-top: 0 !important; }

  .v-tab__slider { height: 2px !important; }

  .v-field__input { max-height: 44px !important; }
</style>