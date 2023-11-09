# json
На стандартный ввод подаются данные о студентах университетской группы в формате JSON:

{
    "ID":134,
    "Number":"ИЛМ-1274",
    "Year":2,
    "Students":[
        {
            "LastName":"Вещий",
            "FirstName":"Лифон",
            "MiddleName":"Вениаминович",
            "Birthday":"4апреля1970года",
            "Address":"632432,г.Тобольск,ул.Киевская,дом6,квартира23",
            "Phone":"+7(948)709-47-24",
            "Rating":[1,2,3]
        },
        {
            // ...
        }
    ]
}
В сведениях о каждом студенте содержится информация о полученных им оценках (Rating). Требуется прочитать данные, и рассчитать среднее количество оценок, полученное студентами группы. Ответ на задачу требуется записать на стандартный вывод в формате JSON в следующей форме:

{
    "Average": 14.1
}
Как вы понимаете, для декодирования используется функция Unmarshal, а для кодирования MarshalIndent (префикс - пустая строка, отступ - 4 пробела).

Если у вас возникли проблемы с чтением / записью данных, то этот комментарий для вас: в уроках об интерфейсах и работе с файлами мы рассказывали, что стандартный ввод / вывод - это файлы, к которым можно обратиться через os.Stdin и os.Stdout соответственно, они удовлетворяют интерфейсам io.Reader и io.Writer, из них можно читать, в них можно писать.

Один из способов чтения, использовать ioutil.ReadAll:

data, err := ioutil.ReadAll(os.Stdin)
if err != nil {
    // ...
}

// data - тип []byte





package main

import (
	"encoding/json"
	"fmt"
	"io"
	"os"
)

type (
	Student struct {
		Rating []float32
	}

	Group struct {
		Students []Student
	}

	Rating struct {
		Average float32
	}
)

func main() {
	var x, y float32
	data, err := io.ReadAll(os.Stdin)
	if err != nil {
		fmt.Println("error")
	}
	var s Group
	json.Unmarshal(data, &s)
	for _, i := range s.Students {
		x += 1
		y += float32(len(i.Rating))
	}
	var a Rating
	a.Average = float32(y) / x
	data2, _ := json.MarshalIndent(a, "", "    ")
	fmt.Printf("%s", data2)
}
