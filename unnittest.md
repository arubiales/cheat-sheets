# 1. Intro
* Importamos Unnittest
* Creamos una clases que herede de "unittest.TestCase"
* todas las funciones de dicha clase que queramos que sean test, deben empezar por "test"
* la función def **"setUp(self)"** la usamos para contener variables o configuración inicial
* la función **"def tearDown(self):"** hace que se corra el setup antes de cada nuevo test, lo que permite configurar las variables iniciales de nuevo
* Dentro de cada función puedes realizar subtest ** for i, test_tz in enumerate(test_timezones):
            with self.subTest(test_name=f'Test # {i}'):
                self.assertNotEqual(tz, test_tz)**
* Otra cosa que podemos hacer es hacer que solo haga raise de el error que le pasemos y el resto los omita, con **"assertRises"**

# 2. Funciones
* assertEqualTo()
* assertTrue()
* assertRaises()
* assertListEqual()
* assertIsNotNone()

# 3. Pytest

 * python -m pytest -v --cov=app: nos da la covertura de nuestro test y el porcentaje pasado de test
 * python -m pytest -v --cov-report=html: nos da una carpeta con un index con el html de nuestra covertura, las lineas cuviertas, etc

## Fixtures
nos sirve como el setup de unnittestt