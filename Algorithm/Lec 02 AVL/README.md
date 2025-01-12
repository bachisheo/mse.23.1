# Деревья поиска 
1. бинарное + АВЛ
2. Декартово
3. Splay

* BST - binary search tree - любое дерево, для которого в каждом узле ключ левого ребенка меньше корня меньше правого


* АВЛ, AVL (Адельсон-Вельский и Ландес) - гарантированный инструемнт, который всегда делает гарантированный алгоритм. Сммотрим на него вместо кчд
* дкартово - как быстрая сортировка - только в среднем и по случанйм питам
* сплей - работает по опреациям: распластанный и широкий 
## Задача поиска 
* input: n элементов с линейным порядком = чист порядок + $\forall x, y x <= y and y <= x$
* query: 


Абстрактный тип данных - что бы мы хотели, чтобы наш агрегат умел делать
* структура данных - реализация АТД

* set - АТД, который умеет делать insert(k), delete(k), find(k) - множество
* map - АТД, который умеет делать insert(k, v), delete(k), find(k) - отображение, словарь

### Хранение в памяти
1. Полное бинарное дерево: как куча на массиве 
2. Неполное бинарное дерево: список записей вида parent, left, right (никаких ограничений)
3. можно любое корневое дерево превратить в бинарное через представление "левый ребенок, правый сосед"
> преимущества: хороша, если ограничена память на каждую вершину (каждая вершина занимает всего 3 памяти)
*картинка*

## BST (binary search tree)
Вершина хранит:
* parent
* left
* right
* key
* h

Правило: все, что слева вершины не больше ее, а справа - не меньше.

T_v - дерево от вершины v, $T_{vleft} <= v.key <= T_{vright}$

## Обход дерева
Будем обходить с помощью dfs с порядком вывода left-root-right
1. Inorder обход
```python
inorder(root r):
    if r == null:
        return
    inorder(r.left)
    print(key)
    inorder(r.right)
    
```
Определение: r - корень дерева поиска <=> inorder обход выводит элементы по неубыванию

Операции:
1. serach: спускаемся по дереву влево-вправо, пока не найдем  
> add -? (через  search)
2. min (самый левый), max(самый правый)
3. next (самый левый в правом поддереве, если такого поддерева нет - если мы левый ребенок - родитель, если правый - идем до первого правого поворота (до первого родителя, в который мы не пришли слева))
4. delete
* лист - удаляем
* один ребенок - заменяем на него (потому что он во всех сранениях ведет себя так же, как родитель (проделывает тот же путь))
* 2 ребенека - заменяем на next или на prev (их нужно честно удалить, т.к. они могут иметь потомков)

Все работает за O($h_t$)

## АВЛ - дерево
Определение: bst сбалансированное, если для каждой вершины v высота его левого и правого ребенка отличаются не больше, чем на 1:
$$h(T.v_{left}) - h(T.v_{right}) <= 1$$
 
> хотим не сильно перекошенное дерево
1. n <= $2^h - 1$ 
2. n >= $1?6^h$
пусть Mh - минимальное число вершин в дереве высоты h. Одно из его поддеревьев точно высоты h-1, его минимальное число вершин M_{h-1}, у второго M_{h-2}, $M_h = M_{h-1} + M_{h-2} + 1$

$N_h = M_h + 1, N_h = N_{h-1} + N_{h-2}  $ - а это  фиббоначи: $F_k >= 1.6^k\cdot C$ 

3. 1, 2 -> h = $\Theta (n)$

## Вращения: малые и большие
> Как выполнять операции над деревьями так, чтобы они оставались сбалансированными?

При операции с вершиной сломаться баланс мог сломаться только на пути от нее до корня
1. Добавили вершину. Пусть левая вершина теперь выше на 2 (больше, чем на 2 различие стать не сможет)

**картинки**

почему это работает? почему не может быть так, что после исправлений у одной из вершин не будет -2?
* пусть мы добавили вершину, тогда у нескольких вершин возможно баланс стал +2 (не больше, т.к. только одно можем добавить)
* если вершина была удалена - только первая вершина на пути испортилась, несколько быть не может. почему? при малом вращении высота осталась такой же, т.к. 

* при удалении баланс нарушится, только если мы удаляем у меньшего дерева
* при удалении "ломается" - т.е. одна зи высот стала маленькой, а та, что и так была больше - осталась такой же, только одна вершина на пути к корнб
* если высота починеной вершины изменилась - может сломаться что-то выше, но снова не больше, чем на 1
