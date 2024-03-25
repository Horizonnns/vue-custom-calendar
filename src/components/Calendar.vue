<script setup>
import { computed, ref } from 'vue';
import IconArrowNext from '../icons/IconArrowNext.vue';
import IconArrowPrev from '../icons/IconArrowPrev.vue';

// monthes
const MONTHSRU = [
	'Январь',
	'Февраль',
	'Март',
	'Апрель',
	'Май',
	'Июнь',
	'Июль',
	'Август',
	'Сентябрь',
	'Октябрь',
	'Ноябрь',
	'Декабрь',
];

const MONTHSEN = [
	'January',
	'February',
	'March',
	'April',
	'May',
	'June',
	'July',
	'August',
	'September',
	'October',
	'November',
	'December',
];

// days
const DAYSRU = ['Пнд', 'Втр', 'Срд', 'Чтв', 'Птн', 'Сбт', 'Вск'];
const DAYSEN = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];

const currLanguage = ref('ru'); // current-language
const currYear = ref(new Date().getFullYear()); // current-year
const currMonthIndex = ref(new Date().getMonth()); // current-month-index

// language-toggler
const languageToggler = () => {
	return (currLanguage.value = currLanguage.value === 'ru' ? 'en' : 'ru');
};

// current-date-(month-&&-year)
const currMonthAndYear = computed(() => {
	return `${
		currLanguage.value === 'ru'
			? MONTHSRU[currMonthIndex.value]
			: MONTHSEN[currMonthIndex.value]
	} ${currYear.value}`;
});

// current-days-of-month-in-array
const currDate = computed(() => {
	let date = new Date(currYear.value, currMonthIndex.value + 1, 0);
	return (date = Array.from({ length: date.getDate() }, (_, i) => i + 1));
});

// first-day-index-of-current-month
const firstDayIndex = computed(() => {
	return new Date(currYear.value, currMonthIndex.value, 1).getDay() - 1;
});

// current-days-of-month
const daysToShow = computed(() => {
	const days = [];

	for (let i = 0; i < firstDayIndex.value; i++) {
		days.push({ index: i, empty: true });
	}

	currDate.value.forEach((day) => {
		days.push({ number: day, empty: false });
	});

	return days;
});

// change-current-month-to-prev
const prevMonth = computed(() => {
	if (currMonthIndex.value === 0) {
		currMonthIndex.value = 11;
		currYear.value--;
	} else {
		currMonthIndex.value--;
	}
});

// change-current-month-to-next
const nextMonth = computed(() => {
	if (currMonthIndex.value === 11) {
		currMonthIndex.value = 0;
		currYear.value++;
	} else {
		currMonthIndex.value++;
	}
});

const isSelected = ref(null);
const formattedDate = ref(null);

// selected-day
const selectedDay = (day) => {
	if (day) {
		isSelected.value = day;

		const selectedDate = new Date(
			currYear.value,
			currMonthIndex.value,
			day + 1
		);
		formattedDate.value = selectedDate.toISOString().split('T')[0];
	} else console.log('empty');
};
</script>

<template>
	<div class="bg-white w-96 p-3 rounded-md shadow-2xl space-y-4">
		<!-- header -->
		<div class="flex items-center justify-between text-lg">
			<button
				@click="languageToggler"
				class="hover:bg-gray-50 active:bg-gray-100 p-1.5 rounded-lg uppercase"
			>
				{{ currLanguage }}
			</button>

			<div class="flex items-center space-x-4">
				<button
					@click="prevMonth"
					class="hover:bg-gray-50 active:bg-gray-100 p-1.5 rounded-lg"
				>
					<IconArrowPrev />
				</button>
				<span>{{ currMonthAndYear }}</span>
				<button
					@click="nextMonth"
					class="hover:bg-gray-50 active:bg-gray-100 p-1.5 rounded-lg"
				>
					<IconArrowNext />
				</button>
			</div>
		</div>

		<!-- calendar-body -->
		<div class="space-y-1">
			<!-- ru-language -->
			<div
				v-if="currLanguage === 'ru'"
				class="flex items-center justify-between"
			>
				<div v-for="day in DAYSRU" :key="day">
					<span>{{ day }}</span>
				</div>
			</div>

			<!-- en-language -->
			<div
				v-if="currLanguage === 'en'"
				class="flex items-center justify-between"
			>
				<div v-for="day in DAYSEN" :key="day">
					<span>{{ day }}</span>
				</div>
			</div>

			<div class="grid grid-cols-7 gap-5">
				<div v-for="day in daysToShow" :key="day.number">
					<button
						@click="selectedDay(day.number)"
						:class="[
							day.empty ? 'bg-gray-200' : '',
							isSelected === day.number ? 'border-blue-500' : '',
						]"
						class="border w-9 h-9"
					>
						{{ day.number }}
					</button>
				</div>
			</div>
		</div>

		<!-- selected-date -->
		<div class="flex justify-center text-lg">
			<div v-if="!formattedDate">
				<span v-if="currLanguage === 'ru'">Дата не выбрано...</span>
				<span v-if="currLanguage === 'en'">Date no selected...</span>
			</div>

			<span>{{ formattedDate }}</span>
		</div>
	</div>
</template>
