# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Гребёнкин Данил Антонович
- ФО-230005
- Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | ? |
| Задание 2 | * | ? |
| Задание 3 | # | ? |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с одной из первых моделей нейросетей - перцептроном и реализовать эту модель в Unity

## Задание 1. В проекте Unity реализовать перцептрон, который умеет производить вычисления
Ход работы:
- Написать скрипт, в котором реализована логика обучения и вычисления перцептрона
```C#  
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour 
{
	public event Action EpochIsCompleted;
	[SerializeField] int epochs;
	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	public double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = UnityEngine.Random.Range(-1.0f,1.0f);
		}
		bias = UnityEngine.Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	public double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train()
	{
		totalError = 0;
		for(int t = 0; t < ts.Length; t++)
		{
			UpdateWeights(t);
			Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
		}
		Debug.Log("TOTAL ERROR: " + totalError);
	}

	IEnumerator TrainCoroutine(int epochs) 
	{
		for (int i = 0; i < epochs; i++)
		{
            yield return new WaitForSeconds(1.0f);
            Train();
            EpochIsCompleted?.Invoke();
        }
		PrintResult();
	}
	void PrintResult()
	{
        Debug.Log("Test 0 0: " + CalcOutput(0, 0));
        Debug.Log("Test 0 1: " + CalcOutput(0, 1));
        Debug.Log("Test 1 0: " + CalcOutput(1, 0));
        Debug.Log("Test 1 1: " + CalcOutput(1, 1));
    }

	void Start () {
        InitialiseWeights();
		StartCoroutine(TrainCoroutine(epochs));
	}
}
```
## Задание 2. Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения
Ход работы:
- Запуск перцептрона
- Запись всех значений в google-таблицу
- Построение графика:
  
![Снимок экрана (2)](https://github.com/user-attachments/assets/3ea3fe7a-ac49-43f9-a317-7f73816196fa)

- Из графика следует, что чем больше количество эпох, тем точнее будет результат вычисления перцептрона.
