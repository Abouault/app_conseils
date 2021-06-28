lancer les test: npm run cypress:open

// syntaxe de base

describe('My First Test', () => {
it('Does not do much!', () => {
// ecrit ton test ici
})
})

1.visiter une url : cy.visit('https://example.cypress.io')

2.le contenu doit contenir : cy.contains('type')

3.cliquer sur un élément : cy.contains('type').click()

4.faire une affirmation : cy.url().should('include', '/commands/actions')

5.entrer du texte sur un élément sélectionner : cy.get('.action-email').type('fake@email.com')

6.Affirmer que l'entrée reflète la nouvelle valeur : .should('have.value', 'fake@email.com')

// action

.blur() - Rendre un élément DOM focalisé flou.
.focus() - Focus sur un élément DOM.
.clear() - Effacez la valeur d'une zone d'entrée ou de texte.
.check() - Cochez la (les) case (s) ou radio (s).
.uncheck() - Décochez la (les) case (s).
.select()- Sélectionnez un <option>dans un <select>.
.dblclick() - Double-cliquez sur un élément DOM.
.rightclick() - Cliquez avec le bouton droit sur un élément DOM.

// login test exemple

exemple :

describe('The Login Page', () => {
beforeEach(() => {
// reset and seed the database prior to every test
cy.exec('npm run db:reset && npm run db:seed')

    // seed a user in the DB that we can control from our tests
    // assuming it generates a random password for us
    cy.request('POST', '/test/seed/user', { username: 'jane.lane' })
      .its('body')
      .as('currentUser')

})

it('sets auth cookie when logging in via form submission', function () {
// destructuring assignment of the this.currentUser object
const { username, password } = this.currentUser

    cy.visit('/login')

    cy.get('input[name=username]').type(username)

    // {enter} causes the form to submit
    cy.get('input[name=password]').type(`${password}{enter}`)

    // we should be redirected to /dashboard
    cy.url().should('include', '/dashboard')

    // our auth cookie should be present
    cy.getCookie('your-session-cookie').should('exist')

    // UI should reflect this user being logged in
    cy.get('h1').should('contain', 'jane.lane')

})
})
