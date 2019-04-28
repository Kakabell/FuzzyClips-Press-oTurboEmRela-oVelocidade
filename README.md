# FuzzyClips-Press-oTurboEmRela-oVelocidade
Trabalho de Inteligência Artificial - Pressão do Turbo em Relação a Velocidade terei tal explosão no motor


(deftemplate Velocidade 
	0 200 Km_h 
	(
		(baixa (z 10 70)) 
		(media (60 0)(80 1)(120 1)(150 0)) 
		(alta (s 145 200)) 
	) 
)

;(plot-fuzzy-value t "-*+" 0 200 (create-fuzzy-value Velocidade baixa) (create-fuzzy-value Velocidade media)(create-fuzzy-value Velocidade alta))


(deftemplate PressaoTurbina 
	0 100 Kilos 
	(
		(fraca (z 1 40)) 
		(media (pi 35 55)) 
		(forte (s 75 100)) 
	) 
)

;(plot-fuzzy-value t "-M+" 0 100 (create-fuzzy-value PressaoTurbina fraca) (create-fuzzy-value PressaoTurbina media) (create-fuzzy-value PressaoTurbina forte))

;Regras

(defrule ExplosaoFraca 
	(declare (salience 10)) 
	(PressaoTurbina fraca) 
	(Velocidade baixa) 
	=> 
	(assert (Explosao ExplosaoFraca)) 
)

(defrule ExplosaoConsideravel 
	(declare (salience 10)) 
	(or (and (PressaoTurbina media) (Velocidade baixa)) 
		(and (PressaoTurbina fraca) (Velocidade media)) 
	) 
	=> 
	(assert (Explosao ExplosaoConsideravel))
)

(defrule ExplosaoMedia 
	(declare (salience 10))
	(or (and (PressaoTurbina forte) (Velocidade baixa))
		(and (PressaoTurbina media) (Velocidade media))
		(and (PressaoTurbina fraca) (Velocidade forte))
	)
	=>
	(assert (Explosao ExplosaoMedia))
)

(defrule ExplosaoMediaAlta 
	(declare (salience 10)) 
		(or (and (PressaoTurbina forte) (Velocidade media)) 
			(and (PressaoTurbina media) (Velocidade forte)) 
		) 
		=> 
		(assert (Explosao ExplosaoMediaAlta)) 
) 

(defrule ExplosaoAlta 
	(declare (salience 10)) 
	(PressaoTurbina forte) (Velocidade forte) 
	=> 
	(assert (Explosao ExplosaoAlta)) 
)


(defglobal ?*g_resultado* = 0 ) 

(defrule defuzifica 
	(declare (salience 0)) 
	?v_tmp <- (Explosao ?) 
	=> 
	(bind ?*g_resultado* (moment-defuzzify ?v_tmp))	
	(plot-fuzzy-value t "*" nil nil ?v_tmp) 
	(retract ?v_tmp) 
	(printout t "Explosao da Turbina: ") 
	(printout t ?*g_resultado* crlf) 
	(printout t " >>> Programa Executado com Sucesso !! <<< " crlf) 
)



;(deffacts fatos (PressaoTurbina fraca) (Velocidade baixa))
