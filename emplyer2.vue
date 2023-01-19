<script setup lang="ts">
import { ref , computed } from 'vue'
import { useHead } from '@vueuse/head'
import packageJson from '../../package.json'
import { isDark, toggleDarkModeHandler } from '/@src/state/darkModeState'
// Start Remove Dark Mode
import { removeDark } from '/@src/state/darkModeState'
removeDark(false)
// End Remove Dark Mode
const faq = [
  {
    title:
      'Can I understand the details about how many candidates viewed my job?',
    content: `After posting your job, you'll see insights on which channels are most effective. You can track how many candidates viewed your ad and how many applications you received from a specific job link. When looking for new talent, you can use this information to better understand where you need to be more visible.`,
  },
  {
    title: 'Can I really use Tenrol for free?',
    content: `Yes, it's completely free! There is, however, a limit of 3 job ads at a time. As your business grows, you can easily upgrade the plan to meet your needs. Please contact us at info@tenrol.com for more information and a comparison of premium products.`,
  },

  {
    title: 'Why are other similar tools 10 times more expensive than Tenrol?',
    content: `The foundation of a great company is a great employee. So we enable companies to attract and manage more applications efficiently, intelligently, and cost-effectively. The tool should be affordable so that every company has access to it.`,
  },
]
type TabId = 'elements' | 'components' | 'forms' | 'plugins'
const activeTab = ref<TabId>('elements')

useHead({
  title: 'Tenrol - The future of hiring',
})
const employerUrl = computed(() =>  {
    return window.location.pathname == "/employer" ;
})
</script>

<template>
  <MinimalLayout theme="light">
    <div class="landing-page-wrapper">
      <!-- HERO -->
      <div class="hero is-fullheight rounded-hero is-active">
        <LandingNavigation />
        <div v-if=" employerUrl" class="hero-body hero-pattern has-text-centered">
            <div class="container">
                <img
                class="header_img"
                src="/@src/assets/parts/hero.png"
                alt="3d man business suite searching for candidates job employment - tenrol.com recruitment solution"
                />

                <h1 class="title is-1 is-bold is-light">
                    <span class="tenrol">Tenrol</span>, the future of hiring
                </h1>
                <h3 class="subtitle is-4 is-light">
                A brilliant solution help you reach and find the best talent on
                the market, every time.
                </h3>
            </div>
        </div>
      </div>
      <!-- sections for recruiter -->
      <LandingRecruiter />

      <!-- FOOTER -->
      <LandingFooter />
    </div>
  </MinimalLayout>
</template>

<style lang="scss">
@import '../scss/abstracts/_mixins.scss';
@import '../scss/pages/demo/_landing.scss';

</style>
