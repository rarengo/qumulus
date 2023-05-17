<template>
  <div class="">
    <v-card variant="outlined" class="elevation-12 rounded-shaped mx-auto mt-8" max-width="300">
      <div class="d-flex align-center flex-column">
      <v-card-text>
        <json-forms
        :data="data"
        :renderers="renderers"
        :schema="schema"
        :uischema="uischema"
        @change="onChange"
        />
      </v-card-text>
        <v-btn :disabled="isButtonDisabled" class="mb-3">Next</v-btn>
      </div>
    </v-card>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { JsonForms } from "@jsonforms/vue";
import {
  vanillaRenderers,
} from "@jsonforms/vue-vanilla";

const renderers = [
  ...vanillaRenderers,
  // here you can add custom renderers
];

const schema = {
    "type": "object",
    "properties": {
    "firstName": {
      "type": "string",
    },
    "lastName": {
      "type": "string"
    },
    "birthday": {
      "type": "string",
      "format": "date"
    },
    "email": {
      "type": "string",
      "format": "email"
    },
    "next": {
      "type": "string",
      "format": "next"
    },
  },
  "required": ["firstName", "lastName", "birthday", "email"],
  "additionalProperties": false
  };

const uischema = {
  "type": "VerticalLayout",
  "elements": [
    {
      "type": "Control",
      "scope": "#/properties/firstName",
      "label": "First Name",
      "options": {
        "variant": "outlined"
      }
    },
    {
      "type": "Control",
      "scope": "#/properties/lastName",
      "label": "Last Name",
      "options": {
        "variant": "outlined"
      }
    },
    {
      "type": "Control",
      "scope": "#/properties/birthday",
      "label": "Birthday",
      "options": {
        "variant": "outlined",
        "type": "date"
      },
      "rule": {
        "effect": "VALIDATE",
        "condition": {
          "type": "jsonforms-expr",
          "expression": "!data.birthday || (new Date().getFullYear() - new Date(data.birthday).getFullYear()) < 18"
        }
      },
    },
    {
      "type": "Control",
      "scope": "#/properties/email",
      "label": "Email",
      "options": {
        "variant": "outlined"
      },
      "rule": {
        "effect": "VALIDATE",
        "condition": {
          "type": "jsonforms-expr",
          "expression": "/.+@.+\\..+/.test(data.email)"
        },
      }
    },
  ]
}

export default defineComponent({
  components: {
    JsonForms,
  },
  data() {
    return {
      // freeze renderers for performance gains
      renderers: Object.freeze(renderers),
      schema,
      uischema,
      isButtonDisabled: true
    };
  },

  methods: {
    onChange(updateData) {
    let datais = updateData
      console.log(datais.data)
    },
  }
});
</script>

