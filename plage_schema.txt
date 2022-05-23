--
-- PostgreSQL database dump
--

-- Dumped from database version 12.0 (Debian 12.0-2.pgdg100+1)
-- Dumped by pg_dump version 12.0 (Debian 12.0-2.pgdg100+1)

SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET xmloption = content;
SET client_min_messages = warning;
SET row_security = off;

--
-- Name: exo_state; Type: TYPE; Schema: public; Owner: plagedba
--

CREATE TYPE public.exo_state AS ENUM (
    'Draft in progress',
    'Need to be tested',
    'Available',
    'Require correction'
);


ALTER TYPE public.exo_state OWNER TO plagedba;

--
-- Name: loc; Type: TYPE; Schema: public; Owner: plagedba
--

CREATE TYPE public.loc AS ENUM (
    'en',
    'fr'
);


ALTER TYPE public.loc OWNER TO plagedba;

SET default_tablespace = '';

SET default_table_access_method = heap;

--
-- Name: acquiredskill; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.acquiredskill (
    user_id integer NOT NULL,
    skill_code character varying(40) NOT NULL
);


ALTER TABLE public.acquiredskill OWNER TO plagedba;

--
-- Name: acquiredskill_user_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.acquiredskill_user_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.acquiredskill_user_id_seq OWNER TO plagedba;

--
-- Name: acquiredskill_user_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.acquiredskill_user_id_seq OWNED BY public.acquiredskill.user_id;


--
-- Name: exercise; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.exercise (
    ex_id integer NOT NULL,
    template_statement text,
    template_archive bytea,
    state public.exo_state NOT NULL,
    author integer NOT NULL,
    name character varying(40) NOT NULL,
    statement_creation_script bytea,
    marking_script bytea,
    locale public.loc NOT NULL,
    ref_id integer
);


ALTER TABLE public.exercise OWNER TO plagedba;

--
-- Name: exercise_ex_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.exercise_ex_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.exercise_ex_id_seq OWNER TO plagedba;

--
-- Name: exercise_ex_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.exercise_ex_id_seq OWNED BY public.exercise.ex_id;


--
-- Name: exerciselevel; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.exerciselevel (
    skill_code character varying(40) NOT NULL,
    ex_id integer NOT NULL,
    nam_id integer NOT NULL
);


ALTER TABLE public.exerciselevel OWNER TO plagedba;

--
-- Name: exerciseproduction; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.exerciseproduction (
    ep_id integer NOT NULL,
    ex_id integer NOT NULL,
    user_id integer NOT NULL,
    comment text,
    is_final boolean NOT NULL,
    score numeric(5,2) NOT NULL,
    processing_log text NOT NULL,
    working_time character varying(40) NOT NULL,
    production_data bytea NOT NULL,
    submissiont_date date NOT NULL
);


ALTER TABLE public.exerciseproduction OWNER TO plagedba;

--
-- Name: exerciseproduction_ep_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.exerciseproduction_ep_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.exerciseproduction_ep_id_seq OWNER TO plagedba;

--
-- Name: exerciseproduction_ep_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.exerciseproduction_ep_id_seq OWNED BY public.exerciseproduction.ep_id;


--
-- Name: nam; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.nam (
    nam_id integer NOT NULL,
    name character varying(40) NOT NULL,
    locale public.loc NOT NULL,
    ref_id integer
);


ALTER TABLE public.nam OWNER TO plagedba;

--
-- Name: nam_nam_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.nam_nam_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.nam_nam_id_seq OWNER TO plagedba;

--
-- Name: nam_nam_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.nam_nam_id_seq OWNED BY public.nam.nam_id;


--
-- Name: plagesession; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.plagesession (
    ps_id integer NOT NULL,
    p_id integer NOT NULL,
    name character varying(40) NOT NULL,
    secret_key character varying(40),
    start_date date,
    end_date date,
    author integer NOT NULL,
    description text,
    universe character varying(40),
    seq_id integer,
    is_timed boolean DEFAULT true NOT NULL
);


ALTER TABLE public.plagesession OWNER TO plagedba;

--
-- Name: plagesession_ps_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.plagesession_ps_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.plagesession_ps_id_seq OWNER TO plagedba;

--
-- Name: plagesession_ps_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.plagesession_ps_id_seq OWNED BY public.plagesession.ps_id;


--
-- Name: profile; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.profile (
    p_id integer NOT NULL,
    job character varying(40) NOT NULL,
    level character varying(40),
    sector character varying(40) NOT NULL,
    description text,
    ref_id integer,
    locale public.loc NOT NULL
);


ALTER TABLE public.profile OWNER TO plagedba;

--
-- Name: profile_p_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.profile_p_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.profile_p_id_seq OWNER TO plagedba;

--
-- Name: profile_p_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.profile_p_id_seq OWNED BY public.profile.p_id;


--
-- Name: profilelevel; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.profilelevel (
    p_id integer NOT NULL,
    skill_code character varying(40) NOT NULL,
    nam_id integer NOT NULL,
    description character varying(40)
);


ALTER TABLE public.profilelevel OWNER TO plagedba;

--
-- Name: profilelevel_p_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.profilelevel_p_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.profilelevel_p_id_seq OWNER TO plagedba;

--
-- Name: profilelevel_p_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.profilelevel_p_id_seq OWNED BY public.profilelevel.p_id;


--
-- Name: sequencelist; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.sequencelist (
    seq_id integer NOT NULL,
    ex_id integer NOT NULL,
    rank integer NOT NULL,
    min_rating numeric(5,2) NOT NULL,
    p_id integer NOT NULL,
    description text
);


ALTER TABLE public.sequencelist OWNER TO plagedba;

--
-- Name: sequencelist_seq_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.sequencelist_seq_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.sequencelist_seq_id_seq OWNER TO plagedba;

--
-- Name: sequencelist_seq_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.sequencelist_seq_id_seq OWNED BY public.sequencelist.seq_id;


--
-- Name: session; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.session (
    sid character varying NOT NULL,
    sess json NOT NULL,
    expire timestamp(6) without time zone NOT NULL
);


ALTER TABLE public.session OWNER TO plagedba;

--
-- Name: skill; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.skill (
    skill_code character varying(40) NOT NULL,
    name character varying(100) NOT NULL,
    th_id integer NOT NULL,
    description text,
    locale public.loc NOT NULL,
    ref_code character varying(40)
);


ALTER TABLE public.skill OWNER TO plagedba;

--
-- Name: studentstatement; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.studentstatement (
    ps_id integer NOT NULL,
    user_id integer NOT NULL,
    ex_id integer NOT NULL,
    availability_date date,
    deadline_date date NOT NULL,
    is_sended boolean DEFAULT false NOT NULL,
    statement text,
    file bytea
);


ALTER TABLE public.studentstatement OWNER TO plagedba;

--
-- Name: theme; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.theme (
    th_id integer NOT NULL,
    name character varying(40) NOT NULL,
    locale public.loc NOT NULL,
    ref_id integer
);


ALTER TABLE public.theme OWNER TO plagedba;

--
-- Name: theme_th_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.theme_th_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.theme_th_id_seq OWNER TO plagedba;

--
-- Name: theme_th_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.theme_th_id_seq OWNED BY public.theme.th_id;


--
-- Name: userplage; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.userplage (
    user_id integer NOT NULL,
    lastname character varying(40) NOT NULL,
    firstname character varying(40) NOT NULL,
    tdgroup character varying(40),
    email character varying(40) NOT NULL,
    enabled boolean NOT NULL,
    role_id integer NOT NULL,
    avatar bytea,
    password character varying(128) NOT NULL,
    organization character varying(40),
    country character varying(40),
    locale public.loc NOT NULL,
    student_number character varying(40),
    nonce character varying(128),
    salt character varying(16) NOT NULL
);


ALTER TABLE public.userplage OWNER TO plagedba;

--
-- Name: userplage_user_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.userplage_user_id_seq
    AS integer
    START WITH 3
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.userplage_user_id_seq OWNER TO plagedba;

--
-- Name: userplage_user_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.userplage_user_id_seq OWNED BY public.userplage.user_id;


--
-- Name: userrole; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.userrole (
    role_id integer NOT NULL,
    name character varying(40) NOT NULL
);


ALTER TABLE public.userrole OWNER TO plagedba;

--
-- Name: userrole_role_id_seq; Type: SEQUENCE; Schema: public; Owner: plagedba
--

CREATE SEQUENCE public.userrole_role_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.userrole_role_id_seq OWNER TO plagedba;

--
-- Name: userrole_role_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: plagedba
--

ALTER SEQUENCE public.userrole_role_id_seq OWNED BY public.userrole.role_id;


--
-- Name: usersession; Type: TABLE; Schema: public; Owner: plagedba
--

CREATE TABLE public.usersession (
    user_id integer NOT NULL,
    ps_id integer NOT NULL
);


ALTER TABLE public.usersession OWNER TO plagedba;

--
-- Name: acquiredskill user_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.acquiredskill ALTER COLUMN user_id SET DEFAULT nextval('public.acquiredskill_user_id_seq'::regclass);


--
-- Name: exercise ex_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exercise ALTER COLUMN ex_id SET DEFAULT nextval('public.exercise_ex_id_seq'::regclass);


--
-- Name: exerciseproduction ep_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciseproduction ALTER COLUMN ep_id SET DEFAULT nextval('public.exerciseproduction_ep_id_seq'::regclass);


--
-- Name: nam nam_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.nam ALTER COLUMN nam_id SET DEFAULT nextval('public.nam_nam_id_seq'::regclass);


--
-- Name: plagesession ps_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.plagesession ALTER COLUMN ps_id SET DEFAULT nextval('public.plagesession_ps_id_seq'::regclass);


--
-- Name: profile p_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profile ALTER COLUMN p_id SET DEFAULT nextval('public.profile_p_id_seq'::regclass);


--
-- Name: profilelevel p_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profilelevel ALTER COLUMN p_id SET DEFAULT nextval('public.profilelevel_p_id_seq'::regclass);


--
-- Name: sequencelist seq_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.sequencelist ALTER COLUMN seq_id SET DEFAULT nextval('public.sequencelist_seq_id_seq'::regclass);


--
-- Name: theme th_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.theme ALTER COLUMN th_id SET DEFAULT nextval('public.theme_th_id_seq'::regclass);


--
-- Name: userplage user_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userplage ALTER COLUMN user_id SET DEFAULT nextval('public.userplage_user_id_seq'::regclass);


--
-- Name: userrole role_id; Type: DEFAULT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userrole ALTER COLUMN role_id SET DEFAULT nextval('public.userrole_role_id_seq'::regclass);


--
-- Name: acquiredskill acquiredskill_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.acquiredskill
    ADD CONSTRAINT acquiredskill_pkey PRIMARY KEY (user_id, skill_code);


--
-- Name: exercise exercise_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exercise
    ADD CONSTRAINT exercise_pkey PRIMARY KEY (ex_id);


--
-- Name: exerciselevel exerciselevel_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciselevel
    ADD CONSTRAINT exerciselevel_pkey PRIMARY KEY (skill_code, ex_id);


--
-- Name: exerciseproduction exerciseproduction_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciseproduction
    ADD CONSTRAINT exerciseproduction_pkey PRIMARY KEY (ep_id);


--
-- Name: nam nam_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.nam
    ADD CONSTRAINT nam_pkey PRIMARY KEY (nam_id);


--
-- Name: plagesession plagesession_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.plagesession
    ADD CONSTRAINT plagesession_pkey PRIMARY KEY (ps_id);


--
-- Name: profile profile_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profile
    ADD CONSTRAINT profile_pkey PRIMARY KEY (p_id);


--
-- Name: profilelevel profilelevel_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profilelevel
    ADD CONSTRAINT profilelevel_pkey PRIMARY KEY (p_id, skill_code);


--
-- Name: sequencelist sequencelist_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.sequencelist
    ADD CONSTRAINT sequencelist_pkey PRIMARY KEY (seq_id, ex_id);


--
-- Name: session session_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.session
    ADD CONSTRAINT session_pkey PRIMARY KEY (sid);


--
-- Name: skill skill_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.skill
    ADD CONSTRAINT skill_pkey PRIMARY KEY (skill_code);


--
-- Name: studentstatement studentstatement_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.studentstatement
    ADD CONSTRAINT studentstatement_pkey PRIMARY KEY (ps_id, user_id, ex_id);


--
-- Name: theme theme_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.theme
    ADD CONSTRAINT theme_pkey PRIMARY KEY (th_id);


--
-- Name: userplage uniqueemail_userplage; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userplage
    ADD CONSTRAINT uniqueemail_userplage UNIQUE (email);


--
-- Name: exercise uniquename_exercise; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exercise
    ADD CONSTRAINT uniquename_exercise UNIQUE (name);


--
-- Name: plagesession uniquename_plagesession; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.plagesession
    ADD CONSTRAINT uniquename_plagesession UNIQUE (name);


--
-- Name: userplage userplage_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userplage
    ADD CONSTRAINT userplage_pkey PRIMARY KEY (user_id);


--
-- Name: userrole userrole_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userrole
    ADD CONSTRAINT userrole_pkey PRIMARY KEY (role_id);


--
-- Name: usersession usersession_pkey; Type: CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.usersession
    ADD CONSTRAINT usersession_pkey PRIMARY KEY (user_id, ps_id);


--
-- Name: IDX_session_expire; Type: INDEX; Schema: public; Owner: plagedba
--

CREATE INDEX "IDX_session_expire" ON public.session USING btree (expire);


--
-- Name: acquiredskill acquiredskill_skill_code_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.acquiredskill
    ADD CONSTRAINT acquiredskill_skill_code_fkey FOREIGN KEY (skill_code) REFERENCES public.skill(skill_code);


--
-- Name: acquiredskill acquiredskill_user_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.acquiredskill
    ADD CONSTRAINT acquiredskill_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.userplage(user_id);


--
-- Name: exercise exercise_author_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exercise
    ADD CONSTRAINT exercise_author_fkey FOREIGN KEY (author) REFERENCES public.userplage(user_id);


--
-- Name: exercise exercise_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exercise
    ADD CONSTRAINT exercise_fk FOREIGN KEY (ref_id) REFERENCES public.exercise(ex_id);


--
-- Name: exerciselevel exerciselevel_ex_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciselevel
    ADD CONSTRAINT exerciselevel_ex_id_fkey FOREIGN KEY (ex_id) REFERENCES public.exercise(ex_id);


--
-- Name: exerciselevel exerciselevel_nam_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciselevel
    ADD CONSTRAINT exerciselevel_nam_id_fkey FOREIGN KEY (nam_id) REFERENCES public.nam(nam_id);


--
-- Name: exerciselevel exerciselevel_skill_code_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciselevel
    ADD CONSTRAINT exerciselevel_skill_code_fkey FOREIGN KEY (skill_code) REFERENCES public.skill(skill_code);


--
-- Name: exerciseproduction exerciseproduction_ex_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciseproduction
    ADD CONSTRAINT exerciseproduction_ex_id_fkey FOREIGN KEY (ex_id) REFERENCES public.exercise(ex_id);


--
-- Name: exerciseproduction exerciseproduction_user_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.exerciseproduction
    ADD CONSTRAINT exerciseproduction_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.userplage(user_id);


--
-- Name: nam nam_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.nam
    ADD CONSTRAINT nam_fk FOREIGN KEY (ref_id) REFERENCES public.nam(nam_id);


--
-- Name: plagesession plagesession_p_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.plagesession
    ADD CONSTRAINT plagesession_p_id_fkey FOREIGN KEY (p_id) REFERENCES public.profile(p_id);


--
-- Name: plagesession plageuser_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.plagesession
    ADD CONSTRAINT plageuser_fk FOREIGN KEY (author) REFERENCES public.userplage(user_id);


--
-- Name: profile profile_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profile
    ADD CONSTRAINT profile_fk FOREIGN KEY (ref_id) REFERENCES public.profile(p_id);


--
-- Name: profilelevel profilelevel_nam_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profilelevel
    ADD CONSTRAINT profilelevel_nam_id_fkey FOREIGN KEY (nam_id) REFERENCES public.nam(nam_id);


--
-- Name: profilelevel profilelevel_p_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profilelevel
    ADD CONSTRAINT profilelevel_p_id_fkey FOREIGN KEY (p_id) REFERENCES public.profile(p_id);


--
-- Name: profilelevel profilelevel_skill_code_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.profilelevel
    ADD CONSTRAINT profilelevel_skill_code_fkey FOREIGN KEY (skill_code) REFERENCES public.skill(skill_code);


--
-- Name: sequencelist sequencelist_ex_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.sequencelist
    ADD CONSTRAINT sequencelist_ex_id_fkey FOREIGN KEY (ex_id) REFERENCES public.exercise(ex_id);


--
-- Name: sequencelist sequencelist_p_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.sequencelist
    ADD CONSTRAINT sequencelist_p_id_fkey FOREIGN KEY (p_id) REFERENCES public.profile(p_id);


--
-- Name: skill skill_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.skill
    ADD CONSTRAINT skill_fk FOREIGN KEY (ref_code) REFERENCES public.skill(skill_code);


--
-- Name: skill skill_th_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.skill
    ADD CONSTRAINT skill_th_id_fkey FOREIGN KEY (th_id) REFERENCES public.theme(th_id);


--
-- Name: studentstatement studentstatement_ex_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.studentstatement
    ADD CONSTRAINT studentstatement_ex_id_fkey FOREIGN KEY (ex_id) REFERENCES public.exercise(ex_id);


--
-- Name: studentstatement studentstatement_ps_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.studentstatement
    ADD CONSTRAINT studentstatement_ps_id_fkey FOREIGN KEY (ps_id) REFERENCES public.plagesession(ps_id);


--
-- Name: studentstatement studentstatement_user_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.studentstatement
    ADD CONSTRAINT studentstatement_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.userplage(user_id);


--
-- Name: theme theme_fk; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.theme
    ADD CONSTRAINT theme_fk FOREIGN KEY (ref_id) REFERENCES public.theme(th_id);


--
-- Name: userplage userplage_role_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.userplage
    ADD CONSTRAINT userplage_role_id_fkey FOREIGN KEY (role_id) REFERENCES public.userrole(role_id);


--
-- Name: usersession usersession_ps_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.usersession
    ADD CONSTRAINT usersession_ps_id_fkey FOREIGN KEY (ps_id) REFERENCES public.plagesession(ps_id);


--
-- Name: usersession usersession_user_id_fkey; Type: FK CONSTRAINT; Schema: public; Owner: plagedba
--

ALTER TABLE ONLY public.usersession
    ADD CONSTRAINT usersession_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.userplage(user_id);


--
-- PostgreSQL database dump complete
--

