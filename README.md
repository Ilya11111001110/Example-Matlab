# Example-Matlab

## Описание
# Скрипт предназначается для розыгрыша призового места в решении олипиады. Розыгрыш числа баллов за задание олимпиады производится с помощью игрального кубика. Критерии выставления баллов берутся из олимпиадного листка. В случае неравномерного распределения числа возможных вариантов оценивая участника олимпиады по количеству возможных выпадений числа на кубике, результат оценивания трактуется в пользу участника. Призовое место в олимпиаде определяется на основе суммарного числа набранных баллов

## Критерии олимпиады 
# За 1 задание можно получить 0,4,7 баллов
# За 2 задание можно получить 0,1,3,4,7 баллов
# За 3 задание можно получить 0,7 баллов
# За 4 задание можно получить 0,3,4,5,7 баллов
# За 5 задание можно получить 0,1,7 баллов

## Исходные данные
n=10;   # число участников
s=zeros(5,1);   # таблица распределения баллов по заданиям для одного участника
res=zeros(n,6); # таблица распределения баллов по заданиям для всех участников + сумма баллов за задание

## Реализация розыгрыша
for k=1:n
    j=1;
    # Розыгрыш числа, выпавшего на грани кубика, для каждого задания олимпиады
    w=zeros(5,1);
    for q=1:5
        w(q)=randi([1 6]);
    end
    # Определение числа баллов в каждом задании и итоговая сумма баллов
    for i=1:5
        switch j
            case 1
                if w(i)<=2
                    s(i)=0;
                elseif w(i)>2 && w(i)<=4
                    s(i)=4;
                else
                    s(i)=7;
                end
                j=j+1;
            case 2
                if w(i)==1
                    s(i)=0;
                elseif w(i)==2
                    s(i)=1;
                elseif w(i)==3
                    s(i)=3;
                elseif w(i)==4
                    s(i)=4;
                else
                    s(i)=7;
                end
                j=j+1;
            case 3
                if w(i)<=3
                    s(i)=0;
                else
                    s(i)=7;
                end
                j=j+1;
            case 4
                if w(i)==1
                    s(i)=0;
                elseif w(i)==2
                    s(i)=3;
                elseif w(i)==3
                    s(i)=4;
                elseif w(i)==4
                    s(i)=5;
                else
                    s(i)=7;
                end
                j=j+1;
            case 5
                if w(i)<=2
                    s(i)=0;
                elseif w(i)>2 && w(i)<=4
                    s(i)=1;
                else
                    s(i)=7;
                end
        end
    end
    Suma=sum(s);
    res(k,1:5)=s;
    res(k,6)=Suma;
end

## Розыгрыш участников
nameMan=["Коля","Илья","Миша","Ваня","Никита","Юра","Степа"]; # имена мальчиков участников
nameFem=["Юля","Катя","Света","Даша","Маша","Аня","Ира"]; # имена девочек участников
surnameMan=["Попов","Смирнов","Петров","Скворцов","Иванов","Терехов","Артемов"]; # фамилии мальчиков участников
surnameFem=["Попова","Смирнова","Петрова","Скворцова","Иванова","Терехова","Артемова"]; # фамилии девочек участников
Wname=[];
Wsurname=[];
for i=1:n
    w=randi([1 2]);
    if w==1
        Wname=[Wname nameMan(randi([1 7]))];
        Wsurname=[Wsurname surnameMan(randi([1 7]))];
    else
        Wname=[Wname nameFem(randi([1 7]))];
        Wsurname=[Wsurname surnameFem(randi([1 7]))];
    end
end
## Представление данных в табличной форме
str=['Competition' '.txt']; # название текстового файла
fileID1=fopen(str,'w+');    # идентификатор
excesize=["Задание 1","Задание 2","Задание 3","Задание 4","Задание 5","Сумма","Место"];
formatSpec2 = '%s \t%s \t%s \t%s \t%s \t%s \t%s\n'; # форма вывода
formatSpec3 = '%2.0f \t%2.0f \t%2.0f \t%2.0f \t%2.0f \t%2.0f\n'; # формат вывода численных данных

fprintf(fileID1,'%s','Количество участников = ');
fprintf(fileID1,'%2.0f\n',(n+1));
for i=1:n
    fprintf(fileID1,'%s',Wname(i));
    fprintf(fileID1,'\t%s\n',Wsurname(i));
    fprintf(fileID1,formatSpec2,excesize);
    fprintf(fileID1,formatSpec3,res(i,:));
    fprintf(fileID1,'%s\n',"");
end
fprintf(fileID1,'%s\n',"Никита Попов");
fprintf(fileID1,formatSpec2,excesize);
fclose(fileID1);
## Управление табличными данными
## Ввод баллов, набранных Никитой
textA=({'Введите баллы за Никиту в текстовый файл по порядку через tab.','Затем нажмите Enter в командном окне для продолжнения'});
textA(1)
textA(2)
pause;
## Обратка всего списка данных
# Чтение данных
fileID2=fopen(str,'r');
data=fscanf(fileID2,'%s');
fclose(fileID2);
# Добавление результатов Никиты в общий список
res((n+1),1)=str2double(data(end-4));
res((n+1),2)=str2double(data(end-3));
res((n+1),3)=str2double(data(end-2));
res((n+1),4)=str2double(data(end-1));
res((n+1),5)=str2double(data(end));
res((n+1),6)=sum(res((n+1),1:5),2);
## Определение победителей
scor=unique(res(:,6)); # сортирует баллы без повторений
# Влючение Никиты в массив списка участников 
Wname=[Wname 'Никита'];
Wsurname=[Wsurname 'Попов'];
# Определение призеров
Win1=[];
Win2=[];
Win3=[];
for i=1:(n+1)
    if res(i,6)==scor(end)
        Win1=[Win1 Wname(i) Wsurname(i)];
    elseif res(i,6)==scor(end-1)
        Win2=[Win2 Wname(i) Wsurname(i)];
    elseif res(i,6)==scor(end-2)
        Win3=[Win3 Wname(i) Wsurname(i)];
    end
end
## Определение места
for i=1:size(scor)
    for j=1:(n+1)
        if res(j,6)==scor(end+1-i)
            res(j,7)=i;
        end
    end
end
## Сводка полученных данных в итоговую таблицу
str=['Competition' '.txt']; # название текстового файла
fileID1=fopen(str,'w+');    # идентификатор
# Количество участников
fprintf(fileID1,'%s','Количество участников = ');
fprintf(fileID1,'%2.0f\n',(n+1));
fprintf(fileID1,'%s\n',"");
# 1 место
fprintf(fileID1,'%s','Первое место:');
fprintf(fileID1,'\t%s %s',Win1);
fprintf(fileID1,'\n%s','Количество баллов = ');
fprintf(fileID1,'%2.0f\n',scor(end));
fprintf(fileID1,'%s\n',"");
# 2 место
fprintf(fileID1,'%s','Второе место:');
fprintf(fileID1,'\t%s %s',Win2);
fprintf(fileID1,'\n%s','Количество баллов = ');
fprintf(fileID1,'%2.0f\n',scor(end-1));
fprintf(fileID1,'%s\n',"");
# 3 место
fprintf(fileID1,'%s','Третье место:');
fprintf(fileID1,'\t%s %s',Win3);
fprintf(fileID1,'\n%s','Количество баллов = ');
fprintf(fileID1,'%2.0f\n',scor(end-2));
fprintf(fileID1,'%s\n',"");
# Вывод данных
formatSpec4='%2.0f \t%2.0f \t%2.0f \t%2.0f \t%2.0f \t%2.0f \t%2.0f\n';
for i=1:(n+1)
    fprintf(fileID1,'%s',Wname(i));
    fprintf(fileID1,'\t%s\n',Wsurname(i));
    fprintf(fileID1,formatSpec2,excesize);
    fprintf(fileID1,formatSpec4,res(i,:));
    fprintf(fileID1,'%s\n',"");
end
fclose(fileID1);
