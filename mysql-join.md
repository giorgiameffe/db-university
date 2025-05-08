# Query con JOIN 

## 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```
SELECT 
    `students`.`name` AS `student_name`,
    `students`.`surname`,
    `students`.`registration_number`,
    `degrees`.`name` AS `degree_name`
FROM
    `students`
        INNER JOIN
    `degrees` ON `degrees`.`id` = `students`.`degree_id`
WHERE
    `degrees`.`name` = 'Corso di Laurea in Economia';
```

## 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```
SELECT 
    `degrees`.`name` AS `degree_name`,
    `degrees`.`level`,
    `departments`.`name` AS `department_name`
FROM
    `degrees`
        INNER JOIN
    `departments` ON `departments`.`id` = `degrees`.`department_id`
WHERE
    `departments`.`name` = 'Dipartimento di Neuroscienze' AND `level` = 'magistrale';
```

## 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```
SELECT 
    `courses`.`name`,
    `courses`.`period`,
    `courses`.`cfu`,
    `teachers`.`id`,
    `teachers`.`name`,
    `teachers`.`surname`
FROM
    `courses`
        INNER JOIN
    `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
        INNER JOIN
    `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE
    `teachers`.`id` = 44;
```

## 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```
SELECT 
    `students`.`name`,
    `students`.`surname`,
    `degrees`.*,
    `departments`.*
FROM
    `students`
        INNER JOIN
    `degrees` ON `degrees`.`id` = `students`.`degree_id`
        INNER JOIN
    `departments` ON `departments`.`id` = `degrees`.`department_id`
ORDER BY 
    `students`.`surname`, `students`. `name`
```

## 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```
SELECT 
    *
FROM
    `degrees`
        INNER JOIN
    `courses` ON `degrees`.`id` = `courses`.`degree_id`
        INNER JOIN
    `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
        INNER JOIN
    `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
```

## 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```
SELECT DISTINCT
    `teachers`.`id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname`,
    `departments`.`name` AS `department_name`
FROM
    `teachers`
        INNER JOIN
    `course_teacher` ON `course_teacher`.`teacher_id` = `teachers`.`id`
        INNER JOIN
    `courses` ON `course_teacher`.`course_id` = `courses`.`id`
        INNER JOIN
    `degrees` ON `courses`.`degree_id` = `degrees`.`id`
        INNER JOIN
    `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE
    `departments`.`name` = 'Dipartimento di Matematica'
ORDER BY `teachers`.`id`;
```

## 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.