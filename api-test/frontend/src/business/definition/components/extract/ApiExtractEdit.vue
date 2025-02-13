<template>
  <div v-loading="loading">
    <div class="extract-item-editing regex" v-if="extract.regex.length > 0">
      <div>
        {{ $t('api_test.request.extract.regex') }}
      </div>
      <div class="regex-item" v-for="(regex, index) in extract.regex" :key="index">
        <ms-api-extract-common
          :if-from-variable-advance="ifFromVariableAdvance"
          :is-read-only="isReadOnly"
          :extract-type="type.REGEX"
          :list="extract.regex"
          :common="regex"
          :edit="true"
          :index="index"
          @savePreParams="savePreParams" />
      </div>
    </div>

    <div class="extract-item-editing json" v-if="extract.json.length > 0">
      <div>JSONPath</div>
      <div class="regex-item" v-for="(json, index) in extract.json" :key="index">
        <ms-api-extract-common
          :if-from-variable-advance="ifFromVariableAdvance"
          :is-read-only="isReadOnly"
          :extract-type="type.JSON_PATH"
          :list="extract.json"
          :common="json"
          :edit="true"
          :index="index"
          @savePreParams="savePreParams" />
      </div>
    </div>

    <div class="extract-item-editing xpath" v-if="extract.xpath.length > 0">
      <div>
        XPath
        <el-select v-model="extract.xpathType" size="mini" v-loading="loading" @change="reload" :disabled="isReadOnly">
          <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
        <el-tooltip placement="top">
          <div slot="content">
            {{ $t('api_test.request.assertions.xpath_info') }}
          </div>
          <i class="el-icon-question" style="cursor: pointer" />
        </el-tooltip>
      </div>
      <div class="regex-item" v-for="(xpath, index) in extract.xpath" :key="index">
        <ms-api-extract-common
          :if-from-variable-advance="ifFromVariableAdvance"
          :is-read-only="isReadOnly"
          :extract-type="type.XPATH"
          :list="extract.xpath"
          :common="xpath"
          :edit="true"
          :index="index"
          @savePreParams="savePreParams" />
      </div>
    </div>
  </div>
</template>

<script>
import { Extract, EXTRACT_TYPE } from '../../model/ApiTestModel';
import MsApiExtractCommon from './ApiExtractCommon';

export default {
  name: 'MsApiExtractEdit',

  components: { MsApiExtractCommon },

  props: {
    extract: {},
    isReadOnly: {
      type: Boolean,
      default: false,
    },
    reloadData: String,
    ifFromVariableAdvance: {
      type: Boolean,
      default: false,
    },
  },
  watch: {
    reloadData() {
      this.reload();
    },
  },

  methods: {
    reload() {
      this.loading = true;
      this.$nextTick(() => {
        this.loading = false;
      });
    },
    savePreParams(data) {
      this.$emit('savePreParams', data);
    },
  },
  data() {
    return {
      type: EXTRACT_TYPE,
      loading: false,
      options: [
        { value: 'html', label: 'html' },
        { value: 'xml', label: 'xml' },
      ],
    };
  },
};
</script>

<style scoped>
.extract-item-editing {
  padding-left: 10px;
  margin-top: 10px;
}

.extract-item-editing.regex {
  border-left: 2px solid #7b0274;
}

.extract-item-editing.json {
  border-left: 2px solid #44b3d2;
}

.extract-item-editing.xpath {
  border-left: 2px solid #e6a200;
}

.regex-item {
  margin-top: 10px;
}
</style>
