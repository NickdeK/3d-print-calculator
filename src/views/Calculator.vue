<template>
  <v-container class="calculator">
    <h1 class="text-h3">Calculator</h1>
    <p>Fill in these fields to get calculate the costs of the 3D print.</p>
    <v-form>
      <div class="mb-4">
        <div class="text-h4">
          Material
          <v-chip v-show="materialCost" outlined>
            €{{ materialCost ? materialCost.toFixed(2) : '' }}
          </v-chip>
        </div>
        <v-row>
          <v-col cols="12" lg="6">
            <v-text-field
              v-model.number="objectWeight"
              label="Object Weight"
              suffix="g"
              placeholder="0"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model="filamentWeight"
              label="Roll/Bottle Weight"
              suffix="g"
              placeholder="1000"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model.number="filamentCost"
              label="Filament Cost"
              prefix="€"
              placeholder="20"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
      </div>

      <div class="mb-4">
        <div class="text-h4">
          Printer usage
          <v-chip v-show="printerCosts" outlined>
            €{{ printerCosts ? printerCosts.toFixed(2) : '' }}
          </v-chip>
        </div>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model.number="printTime"
              label="Print Time"
              suffix="h"
              placeholder="0"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model.number="powerUsagePrinter"
              label="Power Usage Printer"
              suffix="W"
              placeholder="125"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model.number="powerCosts"
              label="Power Costs"
              suffix="kWh"
              prefix="€"
              placeholder="0.21"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
      </div>

      <div class="mb-4">
        <div class="text-h4">
          Labor
          <v-chip v-show="laborCosts" outlined>
            €{{ laborCosts ? laborCosts.toFixed(2) : '' }}
          </v-chip>
        </div>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model.number="startupTime"
              label="Startup Time"
              suffix="h"
              placeholder="0"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
          <v-col cols="6">
            <v-text-field
              v-model.number="cleanupTime"
              label="Cleanup Time"
              suffix="h"
              placeholder="0"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model.number="hourlyRate"
              label="Hourly Rate"
              prefix="€"
              placeholder="1"
              persistent-placeholder
              hide-details="auto"
              type="number"
            />
          </v-col>
        </v-row>
      </div>
    </v-form>

    <div v-show="totalCosts" class="text-h4">
      Total Costs
      <v-chip class="text-overline" color="green">
        €{{ totalCosts ? totalCosts.toFixed(2) : '' }}
      </v-chip>
    </div>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

/** Calculator */
export default defineComponent({
  data: function () {
    return {
      objectWeight: undefined as number | undefined,
      filamentWeight: undefined as number | undefined,
      filamentCost: undefined as number | undefined,
      printTime: undefined as number | undefined,
      powerUsagePrinter: undefined as number | undefined,
      powerCosts: undefined as number | undefined,
      hourlyRate: undefined as number | undefined,
      startupTime: undefined as number | undefined,
      cleanupTime: undefined as number | undefined,
    };
  },
  computed: {
    materialCost: function () {
      if (this.filamentWeight && this.filamentCost && this.objectWeight) {
        return ((this.filamentCost / this.filamentWeight) *
          this.objectWeight) as number;
      }

      return undefined;
    },
    printerCosts: function () {
      if (this.printTime && this.powerUsagePrinter && this.powerCosts) {
        return (
          (this.powerUsagePrinter / 1000) * this.printTime * this.powerCosts
        );
      }

      return undefined;
    },
    laborCosts: function () {
      if (this.hourlyRate && this.startupTime && this.cleanupTime) {
        return ((this.startupTime + this.cleanupTime) *
          this.hourlyRate) as number;
      }

      return undefined;
    },
    totalCosts: function () {
      if (this.laborCosts && this.printerCosts && this.materialCost) {
        return this.laborCosts + this.printerCosts + this.materialCost;
      }

      return undefined;
    },
  },
});
</script>

<style>
.text-h4 {
  margin-bottom: 15px;
}
</style>
