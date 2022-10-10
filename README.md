# Лабораторная №2
Лабораторная №2 для АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ

Отчет по лабораторной работе #1 выполнил(а):
- Голубятникова Ксения Александровна
- РИ210939

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google.Sheets - Unity
- Подключение API для работы с google sheets и google drive

![image](https://user-images.githubusercontent.com/114469025/194910050-1067c8e3-9894-4447-8749-592f24692407.png)
Ссылка на servise account: 
![image](https://user-images.githubusercontent.com/114469025/194910849-bb07c716-d05e-489d-ba73-3d329e7369fa.png)

Ссылка на API Key AIzaSyAnmBIGLQanAGqlkdA3w-n44u6LMxCueJI
![image](https://user-images.githubusercontent.com/114469025/194910807-a26208af-0c9d-48d9-b732-018df02fc061.png)
![image](https://user-images.githubusercontent.com/114469025/194910231-65d162cf-03f8-4c06-9a4d-509fd93fb9cf.png)

- Реализация записи данных из скрипта на python в google-таблицу
Код программы в python
![image](https://user-images.githubusercontent.com/114469025/194911494-e89a95b1-e641-4660-9be0-b632efc63cf4.png)

Вывод данных программы:
![image](https://user-images.githubusercontent.com/114469025/194911614-4c809532-af9f-43c1-bd3a-0189f13c3797.png)

Ссылка на google-таблицу: https://docs.google.com/spreadsheets/d/1WoByECqunJxc9lO_eL8gABwnv5IL4WxzbLaTHig1pr4/edit?usp=sharing

Реализация записи данных из скрипта на python в google-таблицу:
![image](https://user-images.githubusercontent.com/114469025/194911874-dfe5b089-b429-4cdd-9647-9f2b376d0b56.png)

- Написание кода в Unity, в котором воспроизводится файл в зависимости от данных в таблице

Создание проекта на Unity Hub:
![image](https://user-images.githubusercontent.com/114469025/194912698-26d96b12-dd5e-4449-b431-e91acef8e7f6.png)

Код написанный в Visual Studio скрипте проекта:
```py

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip GoodSpeak;
    public AudioClip NormalSpeak;
    public AudioClip BadSpeak;
    private AudioSource selectAudio;
    private Dictionary <string,float> dataSet = new Dictionary<string,float>();
    private bool statusStart = false;
    private int i = 1;
    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest currentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1WoByECqunJxc9lO_eL8gABwnv5IL4WxzbLaTHig1pr4/values/Лист1?key=AIzaSyAnmBIGLQanAGqlkdA3w-n44u6LMxCueJI");
        yield return currentResp.SendWebRequest();
        string rawResp = currentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = GoodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = NormalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = BadSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}

```
Подстановка соответствующих файлов звука:
![image](https://user-images.githubusercontent.com/114469025/194913102-982660cb-a487-4b3c-a1a8-0dbdcb9ee3f3.png)

Вывод консоли в проекте в Unity:


- Сравнение данных:
![image](https://user-images.githubusercontent.com/114469025/194913336-7bb4038f-fe04-4d94-98a9-5900a0346750.png)
![image](https://user-images.githubusercontent.com/114469025/194913364-0253cae0-c1a0-4d0c-8713-8bd882657544.png)
![image](https://user-images.githubusercontent.com/114469025/194913467-ced0b045-1b88-4f98-bd1a-4d19ac919120.png)


## Задание 2
### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
Ход работы:
1. Произвести подготовку данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной 

3. Начать итерацию.

- Шаг 1
