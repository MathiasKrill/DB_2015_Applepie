drop table bz_kunde_gesuch;
drop table bz_kunde_angebot;
drop table strecke_bz;
drop table bz_vermittlung;
drop table angebot;
drop table gesuch;
drop table automobil;
drop table mitarbeiter;
drop table kunde;
drop table ort;

Create table ort(
PLZ           number(5)   constraint plz_pk$ort primary key, --secundarys müssen auf unique oder pk zeige
Ortsname      varchar(50) not null
);

create table Mitarbeiter(
MitarbeiterNr Number(20)  constraint mitarbeiter_pk$mitarbeiter primary key,
Vorname       varchar(50) not null,
nachname      varchar(50) not null,
Straße        varchar(50) not null,
Hausnummer    number(5)   not null,
plz           number(5)   not null,
stadt         varchar(50) not null,
telefonnummer varchar(50)
);

Create table kunde(
KundenNr      Number(20)  constraint kundennr_pk$kunde primary key,
Vorname       varchar(50) not null,
nachname      varchar(50) not null,
Straße        varchar(50) not null,
Hausnummer    number(5)   not null,
plz           number(5)   not null,
stadt         varchar(50) not null,
telefonnummer varchar(50)
);

Create table Automobil(
Modell        varchar(50),
sitze         number(2) not null,
kennzeichen   varchar(50) unique,
besitzer      number(20),-- constraint besitzer_sk references kunde(kundennr)

Constraint besitzer_sk foreign key(besitzer) references Kunde(kundennr)
);


create table Angebot(
AngebotNr     number(20)  constraint angebotNr_pk primary key,
KundeNr       number(20)  constraint kundenNr_sk references Kunde(KundenNr),
Ort_Start     number(5)   constraint ort_start_sk references ort(plz),
Ort_Ueber     number(5)   constraint ort_ueber_sk references ort(plz),
Ort_Ziel      number(5)   constraint ort_ziel_sk  references ort(plz),
Fruehste_Startzeit  Timestamp,
Spaeteste_Startzeit Timestamp,
Sitzplaetze   number(2),
Treffpunkt    varchar(50),
Bemerkung     varchar(50),
Erfasst_von   varchar(50),
Erfasst_am    timestamp,
MitarbeiterNr number(20) constraint mitarbeiter_nr_sk references Mitarbeiter(MitarbeiterNr),

Constraint kunden_nr_con$angebot unique(kundenr)
);

create table Gesuch(
GesuchNr      number(20)  constraint gesuchNr_pk primary key,
KundeNr       number(20)  constraint kundenNr_sk$gesuch references Kunde(KundenNr),
Ort_Start     number(5)   constraint ort_start_sk$gesuch references ort(plz),
Ort_Ziel      number(5)   constraint ort_ziel_sk$gesuch  references ort(plz),
Fruehste_Startzeit  Timestamp,
Spaeteste_Startzeit Timestamp,
Gesuchte_plaetze   number(2),
Treffpunkt    varchar(50),
Bemerkung     varchar(50),
Erfasst_von   varchar(50),
Erfasst_am    timestamp,
MitarbeiterNr number(20) constraint mitarbeiter_nr_sk$gesuch references Mitarbeiter(MitarbeiterNr),
Constraint kunden_nr_con$gesuch unique(kundenr)
);


create table bz_kunde_gesuch(
GesuchNr      number(20) constraint gesuch_nr_sk$bz_gesuch references gesuch(GesuchNr),
KundeNr      number(20) constraint kunde_nr_sk$bz_gesuch references kunde(kundenNr)
);

create table bz_kunde_angebot(
AngebotNr      number(20) constraint angebot_nr_sk$bz_angebot references angebot(angebotNr),
KundeNr      number(20) constraint kunde_nr_sk$bz_angebot references kunde(kundenNr)
);

create table strecke_bz(
Ort_Start     number(5)   constraint ort_start_sk$strecke references ort(plz),
Ort_Ziel      number(5)   constraint ort_ziel_sk$strecke references ort(plz),
km            number(5)
);

create table bz_vermittlung(
AngebotNr     number(20) constraint angebot_nr_sk$bz_vermit references angebot(angebotNr),
GesuchNr      number(20) constraint gesuch_nr_sk$bz_vermit references gesuch(GesuchNr),
Fahrer        number(20) constraint fahre$bz_vermit references Angebot(KundeNr),
Mitfahrer     number(20) constraint mitfahrer$bez_vermit references Gesuch(KundeNr),
Fahrt_done    number(1),
gebuehr       float(20),
Bezahlt_am    timestamp,
Bezahlt_bei   varchar(50),
Vermittelt_von varchar(50),
vermittel_am varchar(50)
);





