<template>
  <section>
    <div class="divCenter">
      <p
        id="works_intro"
        class="lg:text-xl text-md acrlyic w-auto text-center selector"
        ref="worksIntro"
      ></p>
    </div>
    <br />
    <div>
      <h2 class="text-2xl font-bold underline underline-offset-1">Projects</h2>
    </div>
     <div class="flex flex-row justify-end mt-4 ml-[auto]">
      <button
        v-for="(value, index) in typeOfProject"
        :key="index"
        class="px-4 py-3 selector-bg-hover left-border-one text-left text-lg border-l-0 border-b-2 transition ease-in-out delay-100 translate font-bold"
        :class="
          selectedProjectType === value
            ? 'left-border-two border-l-0 border-b-3'
            : null
        "
        @click="changeProjectType(value)"
      >
        {{ value }}
      </button>
    </div>
    <br />
    <div class="flex flex-col divCenter pt-5">
      <!-- works loop -->
      <div
        class="flex 2xl:flex-row flex-col xl:gap-[3rem] gap-[3rem] 2xl:mb-[5rem] mb-[3rem]"
        v-for="(value, index) in filteredWorksData"
        :key="index"
        :class="
          index % 2 !== 0 && !isInMobile
            ? '2xl:flex-row-reverse flex-col-reverse'
            : null
        "
      >
        <div class="animate__animated animate__fadeIn">
          <div class="xl:max-w-[40rem] w-auto 2xl:block hidden">
            <img
              :src="value.Image"
              class="bg-auto bg-center rounded-md border-2 min-w-32"
            />
            <TagsComponent :tagSettings="value.Tags" class="pt-3" />
          </div>

          <div class="2xl:pt-2 xl:max-w-[40rem]">
            <h2
              class="lg:text-xl pb-4 sm:pb-0 text-lg font-bold underline sm:underline-offset-8 decoration-3 sm:decoration-4 whitespace-nowrap"
            >
              {{ value.Title }}
            </h2>

            <div class="xl:max-w-[40rem] w-auto 2xl:hidden flex flex-col">
              <img
                :src="value.Image"
                class="bg-auto bg-center rounded-md border-2"
              />
              <TagsComponent :tagSettings="value.Tags" class="pt-3 pb-3" />
            </div>

            <p class="lg:text-lg text-md text-justify py-2 2xl:py-5">
              {{ value.Description }}
            </p>
            <p
              class="lg:text-lg text-sm text-justify leading-6 2xl:py-2 py-3"
              v-if="value.Sidenote !== ''"
              v-html="value.Sidenote"
            >
            </p>
            <br />
            <strong class="decoration-2 underline"
              ><a :href="value.Url">View this project</a>
            </strong>
            <span v-if="value.SourceCode"> | </span>
            <strong v-if="value.SourceCode" class="decoration-2 underline ml-2"
              ><a :href="value.SourceCode"> View Source Code</a>
            </strong>
            <div v-if="isInMobile" class="mt-3">
              <br />
              <hr
                v-show="index !== indexMaxLength"
                style="border-color: var(--text-highlight)"
              />
            </div>
          </div>
        </div>

       
      </div>
    </div>
  </section>
</template>

<script>
import TypeIt from 'typeit';
import Works from '../components/MainWorksContent.json';
import TagsComponent from '../components/TagsComponent.vue';
import { ref } from 'vue';

export default {
	components: {
		TagsComponent,
	},
	data() {
		return {
			worksData: Works,
			indexMaxLength: 0,
			isInMobile: false,
      selectedProjectType: 'Personal',
      typeOfProject: ['Personal', 'Work']
		};
	},
	setup() {
		const worksIntro = ref('');

		if (worksIntro.value !== '') {
			worksIntro.value === '';
		}
	},
  computed: {
    filteredWorksData() {
      return this.worksData.filter(i => i.Type == this.selectedProjectType.toLowerCase())
    }
  },
	mounted() {
		this.indexMaxLength = this.worksData.length - 1;
		new TypeIt('#works_intro', {
			speed: 50,
			waitUntilVisible: true,
			startDelay: 2000,
			startDelete: true,
			// remove cursor when finished
			afterComplete: function (instance) {
				instance.destroy();
			},
		})
			.type('Here are a list of my projects!')
			.go();

		if (window.innerWidth < 1536) {
			this.isInMobile = true;
		}
	},
  methods: {
    changeProjectType(val) {
      this.selectedProjectType = val
    }
  }
};
</script>

<style></style>
