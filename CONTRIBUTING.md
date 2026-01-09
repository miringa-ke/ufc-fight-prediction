# Contributing to UFC Fight Prediction System

Thank you for your interest in contributing! This project welcomes contributions from everyone.

## 🤝 How to Contribute

### Reporting Bugs

1. Check [existing issues](https://github.com/miringa-ke/ufc-fight-prediction/issues) first
2. Create a new issue with:
   - Clear title
   - Detailed description
   - Steps to reproduce
   - Expected vs actual behavior
   - Screenshots (if applicable)
   - Environment details (Python version, OS, etc.)

### Suggesting Features

1. Open an issue with label `enhancement`
2. Describe the feature and its benefits
3. Explain use cases
4. Discuss implementation approach

### Code Contributions

1. **Fork the repository**
2. **Create a feature branch**:
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Test thoroughly**
5. **Commit with clear messages**:
   ```bash
   git commit -m "Add: Feature description"
   ```
6. **Push to your fork**:
   ```bash
   git push origin feature/amazing-feature
   ```
7. **Open a Pull Request**

## 📝 Development Guidelines

### Code Style

- Follow PEP 8 for Python code
- Use meaningful variable names
- Add comments for complex logic
- Write docstrings for functions

### Testing

- Test your changes locally
- Verify notebooks run end-to-end
- Check API endpoints work
- Ensure no regressions

### Documentation

- Update README.md if needed
- Add docstrings to new functions
- Update API docs for new endpoints
- Include examples in comments

## 🎯 Areas for Contribution

### High Priority

- [ ] Improve model accuracy (current: ~73%)
- [ ] Add more features (fighter age trends, injury history)
- [ ] Optimize API performance
- [ ] Add automated tests
- [ ] Create web interface

### Medium Priority

- [ ] Neural network implementation
- [ ] Predict method of victory (KO, submission, decision)
- [ ] Odds comparison integration
- [ ] Mobile app
- [ ] Docker containerization

### Documentation

- [ ] Video tutorials
- [ ] Blog posts about methodology
- [ ] More code examples
- [ ] Jupyter notebook templates

## 🔬 Project Structure

```
ufc-fight-prediction/
├── data/           # Data files (gitignored)
├── models/         # Trained models (gitignored)
├── notebooks/      # Jupyter notebooks
├── docs/           # Documentation
├── n8n/           # Workflow configs
└── visualizations/ # Charts and plots
```

## 📊 Making Changes

### Adding Features

1. Add to Phase 2 notebook (Feature Engineering)
2. Update feature list
3. Retrain models in Phase 3
4. Document in README

### Improving Models

1. Modify Phase 3 notebook
2. Compare with existing models
3. Update model_metadata.json
4. Document improvements

### API Changes

1. Update Phase 4 notebook
2. Test all endpoints
3. Update API_DOCUMENTATION.md
4. Version the changes

## ✅ Pull Request Checklist

- [ ] Code follows project style
- [ ] All tests pass
- [ ] Documentation updated
- [ ] Commit messages are clear
- [ ] No merge conflicts
- [ ] Screenshots added (for UI changes)

## 🐛 Debugging Tips

- Use `print()` statements liberally
- Check data types with `type()`
- Verify shapes with `.shape`
- Use `try-except` blocks
- Check for NaN values

## 💬 Communication

- Be respectful and constructive
- Ask questions if unclear
- Provide context in discussions
- Help others when you can

## 📜 Code of Conduct

- Be welcoming and inclusive
- Respect differing viewpoints
- Accept constructive criticism
- Focus on what's best for the community

## 🎓 Learning Resources

- [scikit-learn docs](https://scikit-learn.org/)
- [XGBoost guide](https://xgboost.readthedocs.io/)
- [Flask tutorial](https://flask.palletsprojects.com/)
- [n8n documentation](https://docs.n8n.io/)

## 🙏 Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Credited in documentation

Thank you for contributing! 🎉

---

**Questions?** Open an issue or contact [@miringa-ke](https://github.com/miringa-ke)
