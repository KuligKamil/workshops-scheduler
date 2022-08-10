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

let categories = new Set<string>()
for (const i of presentations) categories.add(i.category)
// const groupByCategory: Array<any> = []
const groupByCategory = new Map<string, Presentation[]>()

for (const category of categories) {
  const categoryPresentations = presentations.filter(x => x.category === category)
  groupByCategory.set(category, categoryPresentations)
}

let presenters = new Set<string>()
for (const i of presentations) presenters.add(i.presenter)
// const groupByPresenter: Array<any> = []
const groupByPresenter = new Map<string, Presentation[]>()

for (const presenter of presenters) {
  const presentationsByAuthor = presentations.filter(x => x.presenter === presenter)
  groupByPresenter.set(presenter, presentationsByAuthor)
  // groupByPresenter.push({ name: presenter, presentations: presentationsByAuthor })
}
// let categoriesList = Array.from(categories)
const [category] = categories
const presentationsByCategory: Presentation[] = groupByCategory.get(category)
const results: Array<Presentation> = []
results.push(presentationsByCategory[0])
presentationsByCategory.shift()
groupByCategory.set(category, presentationsByCategory)

for (const result of results) {
  // for (const category of categories) {
  // if (category === result.category) {
  //   continue
  // }
  let presentationsByCategory: Presentation[] = groupByCategory.get(category)
  for (const presentation of presentationsByCategory) {
    if (result.presenter !== presentation.presenter && result.category !== presentation.category) {
      results.push(presentation)
      categoriesList.find(c => c !== category)
    }
  }

  presentationsByCategory = presentationsByCategory.filter((x) => {
    return results[results.length - 1].subject !== x.subject
  })
  groupByCategory.set(category, presentationsByCategory)
  // }
}

// for (const i of presentations)
//   results.push(i)
</script>

<template>
  <main class="px-4 py-10 text-center text-gray-700 dark:text-gray-200">
    <RouterView/>
    <ul>
      <li v-for="presentation of results" :key="presentation.subject">
        {{ presentation.presenter }} -{{ presentation.subject }} [ {{ presentation.category }} ]
      </li>
    </ul>
    <div>before</div>
    <div>before</div>
    <div>before</div>
    <div>before</div>
    <div>before</div>
    <ul>
      <li v-for="presentation of presentations" :key="presentation.subject">
        {{ presentation.presenter }} -{{ presentation.subject }} [ {{ presentation.category }} ]
      </li>
    </ul>
    <Footer/>
    <div class="mt-5 mx-auto text-center opacity-75 dark:opacity-50 text-sm">
      [Home Layout]
    </div>
    <textarea v-model="yml"/>
    <div>{{ yml }}</div>
    <div>{{ groupByCategory }}</div>
    <div>{{ groupByPresenter }}</div>
    <div>aaaaaaa</div>
    <div>{{ results }}</div>
  </main>
</template>
