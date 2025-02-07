<script setup lang="ts">
import type { Content, HTMLRichTextMapSerializer } from '@prismicio/client'

// The array passed to \`getSliceComponentProps\` is purely optional.
// Consider it as a visual hint for you when templating your slice.
defineProps(getSliceComponentProps<Content.HeroSlice>(
  ['slice', 'index', 'slices', 'context']
));
const prismic = usePrismic()

const serializer: HTMLRichTextMapSerializer = {
  ...prismic.options.richTextSerializer,
  heading1: ({ children }) =>
    /* html */ `<h2 class="font-semibold leading-tight tracking-tight md:leading-tight text-5xl md:text-7xl mb-4 mt-12 first:mt-0 last:mb-0">${children}</h2>`,
}
</script>

<template>
  <section class="relative bg-slate-900 text-white">
    <figure class="absolute inset-0">
      <PrismicImage
        v-if="slice.primary.backgroundImage.url"
        :field="slice.primary.backgroundImage"
        class="pointer-events-none select-none object-cover opacity-40 h-full w-full"
      />
    </figure>
    <Bounded
      y-padding="lg"
      class="relative"
    >
      <div class="grid justify-items-center gap-8">
        <PrismicRichText
          :field="slice.primary.text"
          :html-serializer="serializer"
          class="max-w-2xl text-center"
          wrapper="div"
        />
        <PrismicLink
          v-if="slice.primary.buttonLink && ('id' in slice.primary.buttonLink || 'url' in slice.primary.buttonLink)"
          :field="slice.primary.buttonLink"
          class="rounded bg-white px-5 py-3 font-medium text-slate-800"
        >
          {{ slice.primary.buttonText || "Learn More" }}
        </PrismicLink>
      </div>
    </Bounded>
  </section>
</template>
