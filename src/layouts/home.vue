<script setup lang="ts">
import jsonData from '../example.json'

const list: Array<number> = [1, 2, 3]
const data = jsonData.data
const presentations = []

class Presentation {
  presenter: string
  category: string
  subject: string

  constructor(presenter: string, category: string, subject: string) {
    this.category = category
    this.presenter = presenter
    this.subject = subject
  }
}

for (const presenter of data) {
  // console.log(data)
  // console.log(presenter.presenter)
  for (const categories of presenter.categories) {
    for (const presentation of categories.presentations) {
      presentations.push(new Presentation(
        presenter.presenter,
        categories.name,
        presentation,
      ))
    }
  }
}

// let categories = new Set<string>()
// for (const i of presentations) categories.add(i.category)
// // const groupByCategory: Array<any> = []
// const groupByCategory = new Map<string, Presentation[]>()
//
// for (const category of categories) {
//   const categoryPresentations = presentations.filter(x => x.category === category)
//   groupByCategory.set(category, categoryPresentations)
// }
//
// let presenters = new Set<string>()
// for (const i of presentations) presenters.add(i.presenter)
// // const groupByPresenter: Array<any> = []
// const groupByPresenter = new Map<string, Presentation[]>()
//
// for (const presenter of presenters) {
//   const presentationsByAuthor = presentations.filter(x => x.presenter === presenter)
//   groupByPresenter.set(presenter, presentationsByAuthor)
//   // groupByPresenter.push({ name: presenter, presentations: presentationsByAuthor })
// }
// // let categoriesList = Array.from(categories)
// let [category] = categories
// let presentationsByCategory: Presentation[] = groupByCategory.get(category)
// let results: Presentation[] = []
// results.push(presentationsByCategory[0])
// presentationsByCategory.shift()
// groupByCategory.set(category, presentationsByCategory)
// // results = presentations
// // let first
// // [first, category] = categories
//
// function get_presentation(presentationsByCategory: Presentation[], presentationBefore: Presentation) {
//   for (const presentation of presentationsByCategory) {
//     if (presentationBefore.presenter !== presentation.presenter) {
//       return presentation
//     }
//   }
//   return presentationsByCategory[0]
// }
//
// //
// // for (const result of results) {
// //   for (const category of categories) {
// //     let presentationsByCategory: Presentation[] = groupByCategory.get(category)
// //     // const presentation = get_presentation(presentationsByCategory, result)
// //     const presentation = presentationsByCategory[0]
// //     presentationsByCategory = presentationsByCategory.filter((x) => {
// //       return presentation.subject !== x.subject
// //     })
// //     groupByCategory.set(category, presentationsByCategory)
// //     results.push(presentation)
// //   }
// // }
//
// let i = 0
// // console.log(results[i + 1].presenter)
// // console.log(result.presenter)
//
// for (const presentation of presentations) {
//   if (i === 0) {
//     for (const category of categories) {
//       let presentationsByCategory: Presentation[] = groupByCategory.get(category)
//       const presentation = get_presentation(presentationsByCategory, results[i - 1])
//       // const presentation = presentationsByCategory[0]
//       const nextNumber = i + 1
//       if (nextNumber >= 0 && nextNumber < presentations.length) {
//         if (presentation.presenter !== presentations[nextNumber].presenter) {
//           console.log(presentation.presenter)
//           results.push(presentation)
//           presentationsByCategory = presentationsByCategory.filter((x) => {
//             return presentation.subject !== x.subject
//           })
//           groupByCategory.set(category, presentationsByCategory)
//           results.push(presentation)
//         }
//       }
//     }
//   }
//   i++
// }
// for (const i of presentations)
//   results.push(i)
</script>

<template>
  <main class="px-4 py-10 text-center text-gray-700 dark:text-gray-200">
    <RouterView/>
    <!--    <div class="btn">aaaa</div>-->
    <ul>
      <li v-for="presentation of presentations" :key="presentation.subject" class="b-1 b-rd py-4 w-xs">
        {{ presentation.presenter }} -{{ presentation.subject }} [ {{ presentation.category }} ]
        <div class="btn"></div>
      </li>
    </ul>
    <Footer/>
    <div class="mt-5 mx-auto text-center opacity-75 dark:opacity-50 text-sm">
      [Home Layout]
    </div>
  </main>
</template>
