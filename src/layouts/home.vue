<script setup lang="ts">
import jsonData from '../example.json'

const data = jsonData.data
const presentations = []

class Presentation {
  date: string
  presenter: string
  category: string
  subject: string

  constructor(date: string, presenter: string, category: string, subject: string) {
    this.date = date
    this.category = category
    this.presenter = presenter
    this.subject = subject
  }
}

const getDateInFormat = (q: Date) => {
  const ymd = q.toISOString().split('T')[0].split('-')
  return `${ymd[0]}-${ymd[1]}-${ymd[2]}`
}

const disablesDates: Array<string> = []
let basicDate = '2022-09-14'
for (const presenter of data) {
  // console.log(data)
  // console.log(presenter.presenter)

  for (const categories of presenter.categories) {
    for (const presentation of categories.presentations) {
      const d = new Date(basicDate)
      const date = d.toISOString().split('T')[0]
      // this.date = `${d.getMonth()}-${d.getDate()}`
      d.setDate(d.getDate() + 7)
      basicDate = d.toISOString().split('T')[0]
      presentations.push(new Presentation(
        date,
        presenter.presenter,
        categories.name,
        presentation,
      ))
      // disablesDates.push(new Date(`2022-${presentation.date}`))
      disablesDates.push(getDateInFormat(d))
      // disablesDates.push(`${d.getFullYear()}-${d.getMonth()}-${d.getDate()}`)
    }
  }
}
console.log(disablesDates)
const isTaken = ({dayjs}) => {
  console.log(dayjs.format('YYYY-MM-DD'))
  console.log(typeof dayjs.format('YYYY-MM-DD'))
  console.log(disablesDates)
  console.log(disablesDates.includes(dayjs.format('YYYY-MM-DD')))
  return disablesDates.includes(dayjs.format('YYYY-MM-DD'))
}
const date = ref('')
const disabledDate = (time: Date) => {
  // console.log(time)
  console.log()
  console.log(time)
  // time = time.toISOString()
  console.log(getDateInFormat(time))
  console.log(disablesDates.includes(getDateInFormat(time)))
  console.log(disablesDates)
  console.log()
  return disablesDates.includes(getDateInFormat(time))

  // return time.getTime() > Date.now()
}

const handleEdit = (index: number, row: Presentation) => {
  console.log(index, row)
}
const handleDelete = (index: number, row: Presentation) => {
  console.log(index, row)
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

<template lang="pug">
main.dark.px-4.py-10.text-center.text-gray-700(class='dark:text-gray-200')
  h1 Back to school
  el-table(:data='presentations')
    // <el-table-column prop="date" label="Date" width="180"/>
    el-table-column(type='index')
    el-table-column(prop='date' label='Date' width='180')
    el-table-column(prop='presenter' label='Name' width='180')
    el-table-column(prop='subject' label='Subject' width='180')
    el-table-column(prop='category' label='Category')
    el-table-column(align='right')
      template(#header='')
        el-input(v-model='search' size='small' placeholder='Type to search')
      template(#default='scope')
        el-button(size='small' @click='handleEdit(scope.$index, scope.row)') Edit
        el-button(size='small' type='danger' @click='handleDelete(scope.$index, scope.row)') Delete

  RouterView
  Footer
  .block
    span.demonstration Picker with quick options
    el-date-picker(v-model='date' type='date' placeholder='Pick a day' :disabled-date='disabledDate')
      // <div class="btn">aaaa</div>
  ul
    li.b-1.b-rd.py-4.w-xs(v-for='presentation of presentations' :key='presentation.subject')
      | {{ presentation.presenter }} - {{ presentation.subject }} [ {{ presentation.category }} ]
      .btn
  footer
  .mt-5.mx-auto.text-center.opacity-75.text-sm(class='dark:opacity-50')
    | [Home Layout]
</template>
<style scoped>
.cell {
  height: 30px;
  padding: 3px 0;
  box-sizing: border-box;
}

.cell .text {
  width: 24px;
  height: 24px;
  display: block;
  margin: 0 auto;
  line-height: 24px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 50%;
}

.cell.current .text {
  background: #626aef;
  color: #fff;
}

.cell .holiday {
  position: absolute;
  width: 6px;
  height: 6px;
  background: var(--el-color-danger);
  border-radius: 50%;
  bottom: 0px;
  left: 50%;
  transform: translateX(-50%);
}
</style>
