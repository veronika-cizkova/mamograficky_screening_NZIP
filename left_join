/* LEFT JOIN 4 tabulek z NZIP přes 3 sloupce: rok, věkovou kategorii, okres
*/
CREATE OR REPLACE TEMPORARY TABLE "docasna" AS
SELECT "pokryti_populace".*, "pocet_mamo"."pocet_vysetreni" AS "pocet_vysetreni01"
FROM "NZIP_Mamograficky_screening_pokryti_populace" AS "pokryti_populace"
LEFT JOIN "NZIP_Pocet_provedenych_screening_mamo" AS "pocet_mamo" ON "pokryti_populace"."rok" = "pocet_mamo"."rok"
AND "pokryti_populace"."vek_kod" = "pocet_mamo"."vek_kod"
AND "pokryti_populace"."okres_lau_kod" = "pocet_mamo"."okres_lau_kod"
;

CREATE OR REPLACE TEMPORARY TABLE "docasna2" AS
SELECT "d".*, "pz"."pocet_vysetreni" AS "pocet_vysetreni03", "pz"."pocet_vysetreni_doplnujici"
FROM "docasna" AS "d"
LEFT JOIN "NZIP_Podil_zen_s_doplnujicim_vysetrenim_po_mamo" AS "pz" ON "pz"."rok" = "d"."rok"
AND "pz"."vek_kod" = "d"."vek_kod"
AND "pz"."okres_lau_kod" = "d"."okres_lau_kod"
;

UPDATE "docasna2"
SET "vek_nazev" = REPLACE ("vek_nazev", '–', '-')
;

UPDATE "docasna2"
SET "vek_nazev" = SPLIT_PART("vek_nazev", ' ', 1)
;

CREATE OR REPLACE TABLE "final_mamograficky_screening.csv" AS
SELECT "d2".*, "doplnujici_ult"."pocet_uz", "doplnujici_ult"."pocet_mamografii" AS "pocet_mamografii05"
FROM "docasna2" AS "d2"
LEFT JOIN "NZIP_Podil_zen_s_doplnujicim_ult_vysetrenim" AS "doplnujici_ult" ON "doplnujici_ult"."rok" = "d2"."rok"
AND "doplnujici_ult"."vek" = "d2"."vek_nazev"
AND "doplnujici_ult"."okres_lau_kod" = "d2"."okres_lau_kod"
;
