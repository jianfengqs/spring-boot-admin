<!--
  - Copyright 2014-2018 the original author or authors.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->

<template>
  <ul>
    <li
        v-for="instance in instances"
        :key="instance.id"
        :data-testid="instance.id"
        class="flex items-center hover:bg-gray-100 p-2 pr-6"
        @click.stop="showDetails(instance)"
    >
      <div class="pr-3 md:w-16 text-center">
        <sba-status
            :date="instance.statusTimestamp"
            :status="instance.statusInfo.status"
        />
      </div>
      <div class="flex-auto xl:flex-1 xl:w-1/4 truncate">
        <a
            :href="
          instance.registration.serviceUrl || instance.registration.healthUrl
        "
            @click.stop
            v-text="
          instance.registration.serviceUrl || instance.registration.healthUrl
        "
        />
        <sba-tag class="ml-2" v-if="instance.registration.metadata?.['group']"
                 :value="instance.registration.metadata?.['group']" small/>
        <br/>
        <span class="text-sm italic" v-text="instance.id"/>
      </div>
      <div class="hidden xl:block w-1/4 overflow-x-scroll">
        <sba-tags :small="true" :tags="instance.tags" :wrap="false"/>
      </div>
      <div
          class="hidden xl:block w-2/12 text-center"
          v-text="instance.buildVersion"
      />
      <div class="hidden md:block flex-1 text-right">
        <slot :instance="instance" name="actions"/>
      </div>
    </li>
  </ul>
</template>

<script lang="ts" setup>
import Instance from "@/services/instance";
import {useRouter} from "vue-router";
import {PropType} from "vue";

const router = useRouter();

const props = defineProps({
  instances: {
    type: Object as PropType<Array<Instance>>,
    default: () => [],
  },
  showNotificationSettings: {
    type: Boolean,
    default: false,
  },
  hasActiveNotificationFilter: {
    type: Function,
    default: () => false,
  },
})

const showDetails = (instance: Instance) => {
  router.push({
    name: 'instances/details',
    params: {instanceId: instance.id},
  });
}
</script>

<style lang="css">
.instances-list td {
  vertical-align: middle;
}
</style>
