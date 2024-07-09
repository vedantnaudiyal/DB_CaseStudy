# DB_CaseStudy
Part of DB training

## DB MODEL

GIVEN
![WhatsApp Image 2024-07-03 at 10 08 26 AM](https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/c6710e9f-966b-48cf-b2e4-dc969c469450)


## SOLUTION:
### ER-MODEL:
<img width="625" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/adf742e4-0c59-4ae7-ab5d-dc7b0ff5c9a3">


### table query ss
<img width="825" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/af736920-4194-458d-8ea1-4ba5abc121af">



## PROBLEM STATEMENT


<img width="448" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/7be93587-3241-477b-9612-ab38d48d9218">


### SOLUTION ON DB FIDDLE
**Schema (MySQL v8.0)**

    CREATE TABLE IF NOT EXISTS unlabeled_image_predictions (
        image_id int,
        score float
    );
    
    INSERT INTO unlabeled_image_predictions (image_id, score) VALUES
    (18281, 0.31491),
    (17051, 0.98921),
    (1146, 0.56161),
    (594, 0.76701),
    (232, 0.15981),
    (5241, 0.98761),
    (306, 0.6487),
    (11321, 0.88231),
    (19061, 0.83941),
    (12721, 0.97781),
    (1616, 0.10031),
    (1161, 0.71131),
    (1715, 0.89211),
    (11091, 0.1151),
    (1424, 0.77901),
    (1609, 0.52411),
    (1631, 0.25521),
    (12761, 0.26721),
    (17011, 0.0758),
    (554, 0.4418),
    (998, 0.0379),
    (809, 0.1058),
    (1219, 0.71431),
    (402, 0.7655),
    (3631, 0.26611),
    (16241, 0.82701),
    (1640, 0.8790),
    (913, 0.2421),
    (1439, 0.33871),
    (1464, 0.36741),
    (1405, 0.69291),
    (19861, 0.89311),
    (344, 0.37611),
    (847, 0.48891),
    (482, 0.50231),
    (1823, 0.33611),
    (16171, 0.02181),
    (1471, 0.00721),
    (18671, 0.4050),
    (1961, 0.44981),
    (126, 0.35641),
    (943, 0.0452),
    (1115, 0.53091),
    (1417, 0.7168),
    (1706, 0.96491),
    (1166, 0.25071),
    (1991, 0.41911),
    (1465, 0.0895),
    (153, 0.8169),
    (971, 0.9871);

---

**Query #1**

    with cte1 as (select image_id, score, ROW_NUMBER() over (order by score desc) as rowNum from unlabeled_image_predictions),
    cte2 as (select image_id, score, ROW_NUMBER() over (order by score) as rowNum from unlabeled_image_predictions)
    select image_id, round(score) as weak_label from cte1 where rowNum%3=1 and score>0.5 and rowNum<29999
    union
    select image_id, round(score) as weak_label from cte2 where rowNum%3=1 and score<0.5 limit 10000;

| image_id | weak_label |
| -------- | ---------- |
| 17051    | 1          |
| 12721    | 1          |
| 1715     | 1          |
| 19061    | 1          |
| 1424     | 1          |
| 1417     | 1          |
| 1405     | 1          |
| 1115     | 1          |
| 1471     | 0          |
| 943      | 0          |
| 1616     | 0          |
| 232      | 0          |
| 1631     | 0          |
| 18281    | 0          |
| 126      | 0          |
| 18671    | 0          |
| 1961     | 0          |

---

[View on DB Fiddle](https://www.db-fiddle.com/)



