---
layout  : wiki
title   : 프로그래머스 - [3차] 방금그곡
date    : 2023-02-13 16:02:00 +0900
updated : 2023-02-13 16:02:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[[3차] 방금그곡](https://school.programmers.co.kr/learn/courses/30/lessons/17683)

## 구해야 하는 것
- 네오가 찾으려는 음악의 제목

## 주어진 자료
- 네오가 기억하는 멜로디는 한 음악의 끝 부분과 처음 부분이 이어서 재생된 멜로디일 수도 있음
- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공함
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개
- 각 음은 1분에 1개씩 재생됨
- 음악은 반드시 처음부터 재생됨
- 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생됨
- 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생됨
- 음악이 00:00을 넘겨서까지 재생되는 일은 없음
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환함
- 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환함
- 조건이 일치하는 음악이 없을 때에는 "(None)"을 반환함
- 네오가 기억한 멜로디를 담은 문자열 = m
- 방송된 곡의 정보를 담고 있는 배열 = musicinfos

## 조건
- 1 ≤ m을 구성하는 음 개수 ≤ 1439
- musicinfos의 길이 ≤ 100
- musicinfos의 원소는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 ','로 구분된 문자열
- 음악의 시작 시각과 끝난 시각은 24시간 HH:MM 형식
- 음악 제목은 ',' 이외의 출력 가능한 문자로 표현된 문자열
- 1 ≤ 음악 제목 길이 ≤ 64
- 1 ≤ 악보 정보를 구성하는 음 개수 ≤ 1439

## 계획 1
1. 음악의 전체 길이를 구한다.
  - #을 제외한 문자들의 총 개수를 구한다.
2. 실제 방송에서 재생된 전체 멜로디를 구한다.
  - 재생 시간(끝난 시각 - 시작 시각)이 음악 길이보다 짧으면, 멜로디를 재생 시간만큼 자른다.
  - 재생 시간이 음악 길이보다 길면, 초과한 시간만큼 멜로디를 반복한다.
3. 네오가 들은 멜로디와 일치하는 부분이 있는지 확인한다.
  - 일치하는 곡이 많으면 방송된 재생 시간이 제일 긴 음악 제목을 반환한다.
  - 재생 시간도 같으면 먼저 입력된 음악 제목을 반환한다.

## 실행 1 → 실패
```java
class Music {
    String title; // 제목
    String melody; // 음악 전체 멜로디
    int length; // 음악 원래 길이
    int playtime; // 실제 방송된 재생 시간
    String playedMelody; // 실제 방송된 전체 멜로디
}

public int toMinutes(String time) {
    StringTokenizer st = new StringTokenizer(time, ":");
    String hour = st.nextToken();
    String min = st.nextToken();
    int result = Integer.parseInt(hour) * 60 + Integer.parseInt(min);
    return result;
}

public int musicLength(String melody) {
    int result = 0;
    for(int i = 0; i < melody.length(); i++) {
        char current = melody.charAt(i);
        if(current != '#') {
            result++;
        }
    }
    return result;
}

public void setMusics(String[] musicinfos, List<Music> list) {
    for(int i = 0; i < musicinfos.length; i++) {
        StringTokenizer st = new StringTokenizer(musicinfos[i], ",");
        String start = st.nextToken();
        String end = st.nextToken();

        Music m = new Music();
        m.title = st.nextToken();
        m.melody = st.nextToken();
        m.length = musicLength(m.melody);
        m.playtime = toMinutes(end) - toMinutes(start);

        list.add(m);
    }
}

public void getPlayedMelody(List<Music> list) {
    for(int i = 0; i < list.size(); i++) {
        Music current = list.get(i);
        int length = current.length;
        int playtime = current.playtime;
        String melody = current.melody;
        String playedMelody = "";

        if(playtime < length) { // 재생 시간이 음악 길이보다 짧으면
            int index = 0;
            while(playtime > 0) {
                char currentMelody = melody.charAt(index);
                if(currentMelody != '#') {
                    playtime--;
                }
                playedMelody += currentMelody;
                index++;
            }
        } else if(playtime > length) { // 재생 시간이 음악 길이보다 길면
            int repeat = playtime / length; // 노래 반복된 횟수
            int remain = playtime % length; // 반복 후 남은 멜로디 길이

            for(int j = 0; j < repeat; j++) {
                playedMelody += melody;
            }

            int index = 0;

            while (remain > 0) {
                char currentMelody = melody.charAt(index);
                if (currentMelody != '#') {
                    remain--;
                }
                playedMelody += currentMelody;
                index++;
            }
        }
        current.playedMelody = playedMelody;
    }
}

public void getSameMusics(String m, List<Music> list, List<Music> same) {
    for(int i = 0; i < list.size(); i++) {
        String currentMelody = list.get(i).playedMelody;
        if(currentMelody.contains(m)) {
            int index = currentMelody.indexOf(m);
            if(index + m.length() < currentMelody.length()) {
                if(currentMelody.charAt(index + m.length()) != '#') {
                    same.add(list.get(i));
                }
            }
        }
    }
}

public String compare(List<Music> same) {
    if(same.size() == 0) {
        return "(None)";
    }
    int max = -1;
    String answer = "";
    for(int i = 0; i < same.size(); i++) {
        if(same.get(i).playtime > max) {
            max = same.get(i).playtime;
            answer = same.get(i).title;
        }
    }
    return answer;
}

public String solution(String m, String[] musicinfos) {
    List<Music> list = new ArrayList<>();
    List<Music> same = new ArrayList<>();

    setMusics(musicinfos, list);
    getPlayedMelody(list);
    getSameMusics(m, list, same);
    return compare(same);
}
```

## 계획 2
재귀로 바꿀 수 있는 부분들은 재귀로 바꾸고, 범위를 좀더 신경써서 코드를 다시 짜봤다.

## 실행 2
```java
class Music {
    int number; // 재생 순서
    String title; // 제목
    int playtime; // 실제 방송된 재생 시간
    String playedMelody; // 실제 방송된 전체 멜로디
}

class MusicComparator implements Comparator<Music> {
    @Override
    public int compare(Music o1, Music o2) {
        if(o1.playtime > o2.playtime) {
            return 1;
        } else if(o1.playtime < o2.playtime) {
            return -1;
        } else {
            if(o1.number < o2.number) { // 재생 시간이 같으면 재생 순서 순으로 정렬
                return 1;
            } else {
                return-1;
            }
        }
    }
}

// 시간을 분으로 바꾸기
public int toMinutes(String time) {
    StringTokenizer st = new StringTokenizer(time, ":");
    String hour = st.nextToken();
    String min = st.nextToken();
    int result = Integer.parseInt(hour) * 60 + Integer.parseInt(min);
    return result;
}

// 음악 정보 세팅하기
public void setMusics(String[] musicinfos, List<Music> list) {
    int number = 1;
    for(int i = 0; i < musicinfos.length; i++) {
        StringTokenizer st = new StringTokenizer(musicinfos[i], ",");
        String start = st.nextToken();
        String end = st.nextToken();

        Music m = new Music();
        m.number = number;
        m.title = st.nextToken();
        m.playtime = toMinutes(end) - toMinutes(start);
        String melody = st.nextToken();
        m.playedMelody = getPlayedMelody(melody, 0, m.playtime, "");
        list.add(m);
        number++;
    }
}

// 실제 재생된 전체 멜로디 구하기
public String getPlayedMelody(String melody, int index, int playtime, String result) {
    if(playtime == 0) {
        if(index <= melody.length() - 1 && melody.charAt(index) == '#') {
            result += "#";
        }
        return result;
    }

    if(index == melody.length()) {
        return getPlayedMelody(melody, 0, playtime, result);
    }

    char current = melody.charAt(index);
    if(current == '#') {
        return getPlayedMelody(melody, index + 1, playtime, result + current);
    } else {
        return getPlayedMelody(melody, index + 1, playtime - 1, result + current);
    }
}

// 같은 멜로디가 있는 것들만 고르기
public void onlySameMelody(String m, List<Music> list, List<Music> same) {
    for(int i = 0; i < list.size(); i++) {
        Music music = list.get(i);
        String melody = music.playedMelody;

        // 만약 m이 "AB"인데 실제 재생된 멜로디에 "AB#"이 있는 경우 contains에 걸리지 않게 하기 위한 작업
        if(m.charAt(m.length() - 1) != '#') {
            melody = melody.replaceAll(m + "#", " ");
        }

        if(melody.contains(m)) {
            same.add(music);
        }
    }
}

public String getResult(String m, List<Music> list) {
    List<Music> same = new ArrayList<>();
    onlySameMelody(m, list, same); // m이 포함된 음악만 고름
    if(same.size() == 0) { // m이 포함된 음악이 없으면 (None) 출력
        return "(None)";
    }
    Collections.sort(same, new MusicComparator().reversed()); // 재생 시간 순, 재생 순서 순으로 정렬
    return same.get(0).title; // 정렬된 후 제일 앞에 있는 음악의 제목 반환
}

public String solution(String m, String[] musicinfos) {
    List<Music> list = new ArrayList<>();
    setMusics(musicinfos, list);
    return getResult(m, list);
}
```

## 반성
- 실제 코테에서는 시간이 무제한으로 주어지는 것이 아니므로 적당히 풀다가 답을 보려고 했었는데, 이 문제는 왜인지 오기가 생겨서 어떻게든 내가 풀고 싶었고, 결국 풀었다. 문제를 푸는데 필요한 정보만 골라내고, 내가 무슨 일을 해야 하는지 명확하게 계획 세우는 걸 잘 할 수 있게 계속 연습해야 할 것 같다. 복잡한 문제를 마주하면 문제를 작게 쪼개자
- 인덱스를 다룰 때 범위를 잘 확인하자
